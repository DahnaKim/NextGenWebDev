---  
layout: post  
title:  "Express with Database"  
categories: [ Express, AWS ]  
image: assets/img/20241203_01.png  
tags: [featured]  
---  

## AWS RDS와 MySQL연결
### 보안상 Primary Key로 UUID 사용하기를 권장
- 저장 공간과 속도를 위해 UUID_TO_BIN() 및 BIN_TO_UUID() 사용
- 저장 공간 비교 
VARCHAR(36) (텍스트 UUID) : 각 문자마다 1바이트를 사용하므로 총 36바이트 필요
가독성은 높지만, 저장 공간과 비교 속도에서 비효율적
BINARY(16) (바이너리 UUID) : UUID를 16바이트로 저장, 저장 공간을 약 55% 절약, 데이터 비교가 더 빠름
-> 대용량 데이터에서 텍스트 형식 UUID 대신 바이너리 형식 UUID를 사용하면 저장 공간과 성능이 개선됩니다.


### index.js
```
import express from "express";
import {
  getNotesAsync,
  getNoteAsync,
  addNotesAsync,
  updateNoteAsync,
  deleteNoteAsync,
} from "./database.js";

const app = express();
const port = 3000;

app.use(express.json()); // for parsing application/json
app.use(express.urlencoded({ extended: true })); // for parsing application/x-www-form-urlencoded

app.get("/", (req, res) => {
  res.send("Hello World!");
});

app.get("/notes", async (req, res) => {
  const result = await getNotesAsync();
  res.send(result);
});

app.get("/note/:id", async (req, res, next) => {
  try {
    const id = req.params.id;
    if (!id) throw new Error(`400@No path parameter`);
    const result = await getNoteAsync(id);
    if (!result) res.send({});
    if (result.length === 0) res.send({});
    res.send(result[0]);
  } catch (err) {
    next(err);
  }
});

app.post("/notes", async (req, res, next) => {
  const { title, contents } = req.body;
  if (!title) res.sendStatus(400);
  if (!contents) res.sendStatus(400);
  const result = await addNotesAsync(title, contents);
  if (typeof result.affectedRows === "undefined")
    throw new Error(`400@Not created`);
  if (result.affectedRows !== 1) throw new Error(`400@Not created`);
  res.sendStatus(201);
});

app.put("/note/:id", async (req, res) => {
  const { title, contents } = req.body;
  const uuid = req.params.id;
  if (!title) res.sendStatus(400);
  if (!contents) res.sendStatus(400);
  if (!uuid) res.sendStatus(400);
  const result = await updateNoteAsync(uuid, title, contents);
  if (result.affectedRows !== 1) throw new Error(`400@Not updated`);
  res.sendStatus(204);
});

app.delete("/note/:id", async (req, res, next) => {
  try {
    const uuid = req.params.id;
    if (!uuid) throw new Error(`400@No path parameter`);
    const result = await deleteNoteAsync(uuid);
    if (result.affectedRows !== 1) throw new Error(`400@Not deleted`);
    res.sendStatus(204);
  } catch (err) {
    next(err);
  }
});

app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).send("Something broke!");
});

app.listen(port, () => {
  console.log(`Example app listening on port ${port}`);
});
```

### database.js

```
import mysql from "mysql2";

// Create the connection pool. The pool-specific settings are the defaults
const pool = mysql
  .createPool({
    host: "database-1.c4wl7uwjvvru.ap-northeast-1.rds.amazonaws.com",
    user: "admin",
    port: 3306,
    password: "adminadmin",
    database: "db_test",
  })
  .promise();

export async function getNotesAsync() {
  const [rows] = await pool.query(
    `SELECT BIN_TO_UUID(uuid, true) AS uuid,title,contents,created FROM notes;`
  );
  return rows;
}

export async function getNoteAsync(uuid) {
  const [rows] = await pool.query(
    `SELECT BIN_TO_UUID(uuid, true) AS uuid,title,contents,created FROM notes WHERE uuid=UUID_TO_BIN('${uuid}',1);`
  );
  return rows;
}
export async function addNotesAsync(title, contents) {
  const [rows] = await pool.query(
    `INSERT INTO notes (title, contents) VALUES ('${title}', '${contents}');`
  );
  return rows;
}
export async function updateNoteAsync(uuid, title, contents) {
  const [rows] = await pool.query(
    `UPDATE notes SET title = '${title}', contents= '${contents}' WHERE uuid = UUID_TO_BIN('${uuid}',1);`
  );
  return rows;
}
export async function deleteNoteAsync(uuid) {
  const [rows] = await pool.query(
    `DELETE FROM notes WHERE uuid=UUID_TO_BIN('${uuid}',1);`
  );
  return rows;
}
function getNotes() {
  pool.query(
    `SELECT BIN_TO_UUID(uuid, true) AS uuid,title,contents,created FROM notes;`,
    function (err, rows, fields) {
      console.log(rows);
    }
  );
}
function getNote(uuid) {
  pool.query(
    `SELECT BIN_TO_UUID(uuid, true) AS uuid,title,contents,created FROM notes WHERE uuid=UUID_TO_BIN('${uuid}',1);`,
    function (err, rows, fields) {
      console.log(rows);
    }
  );
}
function addNotes(title, contents) {
  pool.query(
    `INSERT INTO notes (title, contents) VALUES ('${title}', '${contents}');
`,
    function (err, rows, fields) {
      console.log(rows);
    }
  );
}
function updateNote(uuid, title, contents) {
  pool.query(
    `UPDATE notes SET title = '${title}', contents= '${contents}' WHERE uuid = UUID_TO_BIN('${uuid}',1);`,
    function (err, rows, fields) {
      console.log(rows);
    }
  );
}
function deleteNote(uuid) {
  pool.query(
    `DELETE FROM notes WHERE uuid=UUID_TO_BIN('${uuid}',1);`,
    function (err, rows, fields) {
      console.log(rows);
    }
  );
}
```

## AWS RDS와 PostgreSQL연결
- AWS설정은 MySQL과 같음
- 강의 섹션 12, 13 참고