# AWS S3 버킷에 파일을 업로드 및 접근 방법

> 환경  
> node, react, express, aws_s3

## STEP 1: AWS 액서스 키 발행

<img width="811" alt="AWS 액서스 키 발행" src="https://user-images.githubusercontent.com/14961047/62129619-1edb9100-b312-11e9-94b1-f1a206699691.png">

## STEP 2: AWS S3 권한 설정

<img width="979" alt="AWS S3 권한 설정" src="https://user-images.githubusercontent.com/14961047/62129924-d5d80c80-b312-11e9-8662-b019a06286be.png">

퍼블릭 액서스를 모두 예로 결정한다.

## STEP 3: Back End

### 1. 노드 모듈을 설치한다

```bash
yarn add aws-sdk --save

yarn add dotenv --save
```

### 2. 환경 변수 설정

루트 디렉터리 `.env` 파일에 aws 접근과 관련된 액서스키를 환경변수로 저장해준다.

깃허브에 올라가지 않도록 `.gitignore` 파일에 `.env` 코드가 없다면 삽입해준다.

```
aws_access_key_id=발급받은 액서스 키

aws_secret_access_key=발급받은 보호 액서스 키

aws_bucket=S3버킷 이름
```

### 3. Controller 작성

`routes/controllers`디렉터리에 `media.js` 파일을 생성하고 아래 코드를 작성한다

```js
// routes/controllers/media.js

const express = require("express");
const router = express.Router();
const AWS = require("aws-sdk");
require("dotenv").config(); // Configure dotenv to load in the .env file
// Configure aws with your accessKeyId and your secretAccessKey
AWS.config.update({
  region: "ap-northeast-2", // Put your aws region here
  accessKeyId: process.env.aws_access_key_id,
  secretAccessKey: process.env.aws_secret_access_key
});

const S3_BUCKET = process.env.aws_bucket;
// Now lets export this function so we can call it from somewhere else

router.post("/sign_s3", (req, res) => {
  const s3 = new AWS.S3(); // Create a new instance of S3
  const fileName = req.body.fileName;
  const fileType = req.body.fileType;
  // Set up the payload of what we are sending to the S3 api
  const s3Params = {
    Bucket: S3_BUCKET,
    Key: fileName,
    Expires: 500,
    ContentType: fileType,
    ACL: "public-read"
  };

  // Make a request to the S3 API to get a signed URL which we can use to upload our file
  s3.getSignedUrl("putObject", s3Params, (err, data) => {
    if (err) {
      console.log(err);
      res.json({ success: false, error: err });
    }

    // Data payload of what we are sending back, the url of the signedRequest and a URL where we can access the content after its saved.
    const returnData = {
      signedRequest: data,
      url: `https://${S3_BUCKET}.s3.amazonaws.com/${fileName}`
    };
    // Send it all back
    res.json({ success: true, data: { returnData } });
  });
});

module.exports = router;
```

### 4. 라우팅

루트 디렉터리 server.js 파일에 controller 접근을 위해 미들웨어 설정하여 라우팅시킨다

```js
// server.js

app.use("/api/controllers", require("./routes/controllers/media"));
```

## STEP 4: REACT Component

`client/components/layout`디렉터리를 생성하고 `Upload_file.js` 파일을 아래와 같이 작성한다.

```js
import React, { Component } from "react";
import axios from "axios";

class Upload_file extends Component {
  constructor(props) {
    super(props);
    this.state = {
      success: false,
      url: ""
    };
  }

  handleChange = ev => {
    this.setState({ success: false, url: "" });
  };

  // Perform the upload
  handleUpload = ev => {
    let file = this.uploadInput.files[0];
    // Split the filename to get the name and type
    let fileParts = this.uploadInput.files[0].name.split(".");
    let fileName = fileParts[0];
    let fileType = fileParts[1];
    console.log("Preparing the upload");
    axios
      .post("/api/controllers/sign_s3", {
        fileName: fileName,
        fileType: fileType
      })
      .then(response => {
        var returnData = response.data.data.returnData;
        var signedRequest = returnData.signedRequest;
        var url = returnData.url;
        this.setState({ url: url });
        console.log("Recieved a signed request " + signedRequest);

        // Put the fileType in the headers for the upload
        var options = {
          headers: {
            "Content-Type": fileType
          }
        };
        axios
          .put(signedRequest, file, options)
          .then(result => {
            console.log("Response from s3");
            this.setState({ success: true });
          })
          .catch(error => {
            alert("ERROR " + JSON.stringify(error));
          });
      })
      .catch(error => {
        alert(JSON.stringify(error));
      });
  };

  render() {
    const Success_message = () => (
      <div style={{ padding: 50 }}>
        <h3 style={{ color: "green" }}>SUCCESSFUL UPLOAD</h3>
        <a href={this.state.url}>Access the file here</a>
        <br />
      </div>
    );
    return (
      <div className="Upload_file">
        <center>
          <h1>UPLOAD A FILE</h1>
          {this.state.success ? <Success_message /> : null}
          <input
            onChange={this.handleChange}
            ref={ref => {
              this.uploadInput = ref;
            }}
            type="file"
          />
          <br />
          <button onClick={this.handleUpload}>UPLOAD</button>
        </center>
      </div>
    );
  }
}

export default Upload_file;
```

## STEP 5: 실행 및 확인

<img width="407" alt="Successful upload" src="https://user-images.githubusercontent.com/14961047/62131067-67487e00-b315-11e9-951c-d9e1171e885b.png">

<img width="971" alt="AWS S3 버킷 확인" src="https://user-images.githubusercontent.com/14961047/62131244-c0181680-b315-11e9-88a7-1c74ab1d2cff.png">
파일이 정상적으로 올라간 것을 확인 할 수 있다.

---

## 참조

<https://medium.com/@khelif96/uploading-files-from-a-react-app-to-aws-s3-the-right-way-541dd6be689>
