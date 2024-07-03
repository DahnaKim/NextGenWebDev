---    
layout: post  
title:  "Social Authentication2"  
categories: [ Clone Coding ]  
image: assets/img/20240702_01.png  
tags: [featured]  
---  
  
# Social Authentication2  
  
```  
"use server";  
  
import twilio from "twilio";  
import crypto from "crypto";  
import { z } from "zod";  
import validator from "validator";  
import { redirect } from "next/navigation";  
import db from "@/lib/db";  
import getSession from "@/lib/session";  
  
const phoneSchema = z  
  .string()  
  .trim()  
  .refine(  
    (phone) => validator.isMobilePhone(phone, "en-US"),  
    "Wrong phone format"  
  );  
  
async function tokenExists(token: number) {  
  const exists = await db.sMSToken.findUnique({  
    where: {  
      token: token.toString(),  
    },  
    select: {  
      id: true,  
    },  
  });  
  return Boolean(exists);  
}  
  
const tokenSchema = z.coerce  
  .number()  
  .min(100000)  
  .max(999999)  
  .refine(tokenExists, "This token does not exist.");  
  
interface ActionState {  
  token: boolean;  
}  
  
async function getToken() {  
  const token = crypto.randomInt(100000, 999999).toString();  
  const exists = await db.sMSToken.findUnique({  
    where: {  
      token,  
    },  
    select: {  
      id: true,  
    },  
  });  
  if (exists) {  
    return getToken();  
  } else {  
    return token;  
  }  
}  
  
export async function smsLogIn(prevState: ActionState, formData: FormData) {  
  const phone = formData.get("phone");  
  const token = formData.get("token");  
  if (!prevState.token) {  
    const result = phoneSchema.safeParse(phone);  
    if (!result.success) {  
      return {  
        token: false,  
        error: result.error.flatten(),  
      };  
    } else {  
      await db.sMSToken.deleteMany({  
        where: {  
          user: {  
            phone: result.data,  
          },  
        },  
      });  
      const token = await getToken();  
      await db.sMSToken.create({  
        data: {  
          token,  
          user: {  
            connectOrCreate: {  
              where: {  
                phone: result.data,  
              },  
              create: {  
                username: crypto.randomBytes(10).toString("hex"),  
                phone: result.data,  
              },  
            },  
          },  
        },  
      });  
      const client = twilio(  
        process.env.TWILIO_ACCOUNT_SID,  
        process.env.TWILIO_AUTH_TOKEN  
      );  
      await client.messages.create({  
        body: `Your Karrot verification code is: ${token}`,  
        from: process.env.TWILIO_PHONE_NUMBER!,  
        to: process.env.MY_PHONE_NUMBER!,  
      });  
  
      return {  
        token: true,  
      };  
    }  
  } else {  
    //phone값 확인  
    const result = await tokenSchema.spa(token);  
    if (!result.success) {  
      return {  
        token: true,  
        error: result.error.flatten(),  
      };  
    } else {  
      //phone값 확인  
      const token = await db.sMSToken.findUnique({  
        where: {  
          token: result.data.toString(),  
        },  
        select: {  
          id: true,  
          userId: true,  
        },  
      });  
      const session = await getSession();  
      session.id = token!.userId;  
      await session.save();  
      await db.sMSToken.delete({  
        where: {  
          id: token!.id,  
        },  
      });  
      redirect("/profile");  
    }  
  }  
}  
```  
  
phoneSchema: 전화번호가 유효한 미국 휴대폰 번호인지 검증합니다. zod와 validator를 사용하여 입력된 전화번호가 유효한 형식인지 확인합니다.  
  
tokenExists: 주어진 토큰이 데이터베이스에 존재하는지 확인합니다.  
  
tokenSchema: 토큰이 유효한 숫자 범위 내에 있으며, 데이터베이스에 존재하는지 검증합니다.  
  
getToken: 새로운 토큰을 생성합니다. 생성된 토큰이 이미 데이터베이스에 존재하면 다시 생성합니다.  
  
smsLogIn: SMS 로그인 절차를 처리합니다. formData에서 전화번호와 토큰을 가져와서 검증 및 토큰 발급, 인증을 진행합니다.  
* 초기 상태 (prevState.token이 false일 때):  
    * 입력된 전화번호를 검증합니다.  
    * 전화번호가 유효하지 않으면 오류를 반환합니다.  
    * 기존의 SMS 토큰을 삭제하고 새로운 토큰을 생성하여 데이터베이스에 저장합니다.  
    * Twilio를 사용하여 사용자에게 SMS로 인증 토큰을 보냅니다.  
* 토큰이 있는 상태 (prevState.token이 true일 때):  
    * 입력된 토큰을 검증합니다.  
    * 토큰이 유효하지 않으면 오류를 반환합니다.  
    * 유효한 토큰이면 사용자 세션을 생성하고, 인증된 후 프로필 페이지로 리디렉션합니다.  
  