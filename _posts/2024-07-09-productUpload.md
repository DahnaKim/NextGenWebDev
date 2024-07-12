---   
layout: post  
title:  "Product Upload"  
categories: [ Clone Coding ]  
image: assets/img/20240709_03.png  
tags: [featured]  
---  
  
# Product Upload  
  
**변경파일**  
- app/(tabs)/products/page.tsx  
- app/products/add/page.tsx  
- components/product-list.tsx  
  
![1]({{ site.baseurl }}/assets/img/20240709_01.png)  
  
<br>  
  
## Form Action  
**변경파일**  
- app/products/add/actions.ts  
- app/products/add/page.tsx  
  
`URL.createObjectURL(file);` : URL을 생성  
  
<br>  
  
## Product Upload  
**변경파일**  
- app/products/add/actions.ts  
- app/products/add/page.tsx  
  
<br>  
  
## Images Setup  
  
### AWS S3 vs. Cloudflare  
AWS S3 : 그냥 URL만 줌  
Cloudflare : 이미지에 최적화. 이미지를 변형할 수 있음(ex: 이미지를 50% 품질로 요청하는 등)   
**Cloudflare Images 사용하는 경우**  
- token을 생성하고 복사해서 .env에 넣기  
- 결제 후 Cloudflare Account ID, Account Hash복사해서 .env에 넣기  
  
<br>  
  
## Upload URLs  
  
![2]({{ site.baseurl }}/assets/img/20240709_02.png)  
  
**변경파일**  
- app/products/add/actions.ts  
- app/products/add/page.tsx  
- .env  
  
<br>  
  
## Image Upload  
**변경파일**  
- app/products/[id]/page.tsx  
- app/products/add/page.tsx  
- components/list-product.tsx  
- next.config.mjs  
  
![3]({{ site.baseurl }}/assets/img/20240709_03.png)  
  
<br>  
  
## Variants  
**변경파일**  
- app/products/[id]/page.tsx  
- components/list-product.tsx  
  
![4]({{ site.baseurl }}/assets/img/20240709_04.png)  
  
<br>  
  
## RHF Refactor  
`React Hook Form` : 예전에는 모두가 기본으로 썼으나 Next.js 최신 버전으로 넘어가면서 필수가 아닌 선택지로 바뀜. 강의에서는 이미 사용하고 있고 함께 사용하는 방법을 알려주기 위해 사용  
`npm i react-hook-form`  
`npm i @hookform/resolvers` : Zod schema를 사용해서 form을 validation할 수 있게 해줌  
**변경파일**  
- app/products/add/actions.ts  
- app/products/add/page.tsx  
- app/products/add/schema.ts  
  
<br>  
  
## Recap  
**변경파일**  
- app/products/add/page.tsx  
- components/input.tsx  
- react hook-form은 ref(reference)를 사용함 => forwardRef() 사용  