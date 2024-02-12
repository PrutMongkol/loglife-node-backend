# LogLife APIs

## Overview

APIs for accessing database of LogLife web application.

Access via <https://jsd6-loglife-backend.onrender.com>

## API Endpoints

| HTTP Verbs | Endpoints                | Action                                    |
| ---------- | ------------------------ | ----------------------------------------- |
| **GET**    | /activities/user/:userId | To **retrieve all activities** of a user  |
| **GET**    | /activities/:activityId  | To **retrieve each activity** information |
| **POST**   | /activities              | To create a new activity                  |
| **PUT**    | /activities/:activityId  | To edit information on each activity      |
| **DELETE** | /activities/:activityId  | To delete a single activity               |

## Query Parameters

for `GET` on `/activities/user/:userId`

| Query Params | Value                                                     | Action                                                        |
| ------------ | --------------------------------------------------------- | ------------------------------------------------------------- |
| **?type**    | `null`                                                    | To retrieve activities **from all types**                     |
|              | `Running`, `Cycling`, `Swimming`, `Walking`, `Hiking`, `Other` | To retrieve activities **from each type**                     |
| **?sort**    | `null`   / `date-desc`                                                  | To retrieve all activities **descending by date + startTime** |
|              | `date-asc`                                                | To retrieve all activities **ascending by date + startTime** |
| **?skip**    | `null`                                                    | To skip 0 entry
|              | *number*                                                  | To skip *[number]* of entries                                      |
| **?take**    | `null`                                                    | To take *20* entries                          |
|              | *number*                                                  | To take *[number]* of entries                          |

## Request Body

for `POST` and `PUT` endpoints

```
{
  "userId": string
  "title": string,
  "description": string,
  "type": string,
  "startTime": string,
  "endTime": string,
  "date": string,
  "duration": {
    "hour": number,
    "minute": number
  },
  "barometer": string
}
```

## Response Body

### GET
>
> #### All Activities
>
> ```
> [
>  {
>    "activityId": string,
>    "userId": string
>    "title": string,
>    "description": string,
>    "type": string,
>    "startTime": string,
>    "endTime": string,
>    "date": string,
>    "duration": {
>      "hour": number,
>      "minute": number
>    },
>    "barometer": string
> },
>  {
>    ...
>  }
>]
>```

> #### Single Activity
>
> ```
>  {
>    "activityId": string,
>    "userId": string
>    "title": string,
>    "description": string,
>    "type": string,
>    "startTime": string,
>    "endTime": string,
>    "date": string,
>    "duration": {
>      "hour": number,
>      "minute": number
>    },
>    "barometer": string
> }
>```
>
### POST
>
>```
>{
>  "result": {
>    "acknowledged": boolean,
>    "insertedId": string
>  }
>}
>```

### PUT
>
>```
>{
>"result": {
>    "acknowledged": boolean,
>    "modifiedCount": int,
>    "upsertedId": string | null,
>    "upsertedCount": int,
>    "matchedCount": int
>  }
>}
>```
>
### DELETE
>
>```
>{
>  "result": {
>    "acknowledged": boolean,
>    "deletedCount": int
>  }
>}
>```

## Response Code

| HTML Code | Explaination | Included Message |
|------|-------|------|
| **200** | OK | *Response body* |
|**201** | Entry Created | *Response body* |
| **400** | Entries Not Found | `Invalid userId`, `Invalid activityId`, *Missing field details* |
| **500** | Internal Error | Backend error message(s)


## Authetication
**Library**
 - jsonwebtoken
 - cookie-parser
 - bcrypt

**setup**

    webServer.use(cors({
    
    origin: true,
    
    credentials: true,
    
    }));
  เพื่ออนุญาตให้ domain อื่นส่ง cookie มายังเชิร์ฟเวอร์ได้
  ### token
  การสร้าง token 
 

    const  jwtSecretKey  =  process.env.TOKEN_KEY;
    
    const  token  = jwt.sign({ id: tokenvalue }, jwtSecretKey, {
    
    expiresIn: "2h",
    
    });
   การ save token ลงบน cookie
  

    const  token  =  createToken(userId);
    
    res.status(201).cookie('loglife',token,{
    
	    maxAge: 300000,
    
	    secure: true,
    
	    httpOnly: true,
    
	    sameSite: "none",
    
    })
   **ค่าที่ส่งกลับไป**
   

 **1. message**
 **2. user**
 
 **ค่าใน user ประกอบด้วย**
 - first_name
 - last_name  
 - emailAddress 
 - userId

 

   
  ###  FrontEnd
  #### Response

   **response**

    {
    
	    message:'Signup success',
    
	    user:userId
    
    }
   **ค่าที่เก็บใน userId**

     { projection:
    
    { userId: "$_id",
    
    _id:0,
    
    first_name:1,
    
    last_name:1,
    
    emailAddress:1,
    
    }
    
    }

  ตัวอย่างการส่งค่าไปยัง backend 
 

    const  response  =  await  axios.post("http://localhost:3000/login", {
    
	    emailAddress,
    
	    password,
    
    }, {
    
	    withCredentials: true
    
    });
   ตัวอย่างการนำค่าไปใช้งาน
 

    console.log("message: ",response.data.message);
    
    console.log("first_name: ",response.data.user.first_name);
    
    console.log("last_name: ",response.data.user.last_name);
    
    console.log("emailAddress: ",response.data.user.emailAddress);
    
    console.log("UserId: ",response.data.user.userId);