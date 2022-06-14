# DEMO: For Beginners (zero coding experience)

## Data Preparation (Step 1 to 4)

<details>
  <summary>Step 1: Deploy Label-Studio</summary>

### Step 1: Deploy Label-Studio

1. Sign up on Heroku: https://signup.heroku.com/ and verify your email.
2. Click this button: [<img src="https://www.herokucdn.com/deploy/button.svg" height="30px">](https://heroku.com/deploy?template=https://github.com/heartexlabs/label-studio/tree/heroku-persistent-pg).
3. Pick any name for the app (e.g., `label-studio-0`).
4. Change `DISABLE_SIGNUP_WITHOUT_LINK` from `0` to `1`. For `USERNAME`, type your **email address**.
5. Enter a username and password for the default account that you will use for label-studio.
6. Click `deploy`!

When your `label-studio` app is deployed, you will see something that like this:

![ls_deploy_ok](https://i.imgur.com/X8NuIkk.png)

Click on `View`, then login with the username and password you entered earlier.

</details>

---

<details>
  <summary>Step 2: Create a cloud storage</summary>
  
### Step 2: Create a cloud storage

You would wanna keep your data in the cloud so it's easily moved between different apps. For that, you will create a **S**imple **S**torage **S**ervice (S3) bucket, where all your data will be kept.
There are a lot of options for S3-compatible object storage service, but here, we will use Wasabi.

1. Sign up on Wasabi: https://wasabi.com/sign-up/ and verify your email.
2. Open the console: https://console.wasabisys.com, then click on `Buckets` -> `CREATE BUCKET`.

![create_bucket](https://i.imgur.com/9Sxl8tg.png)

3. Pick up a name for your bucket (e.g., `data-0`), select a region (e.g., `us-east-1`), then click `Next`.
4. Keep the options as they are, then click `Next`, then click `CREATE BUCKET`.

That's it! You made an S3 bucket! Now leave the page open, because we will go back to it later to upload our data.

</details>

---

<details>
  <summary>Step 3: Create a backend database</summary>
  
### Step 3: Create a backend database

This database will be used to store all the information related to the data generated by the project. We will use MongoDB as a backend database. You can create one for free with plenty of storage for what we need.

1. Sign up on MongoDB Atlast: https://www.mongodb.com/cloud/atlas/register and verify your email.
2. Visit https://cloud.mongodb.com and click on `Build a Database` -> select `Shared` -> `Create`.
3. Keep the options as they are. You can change the cluster name if you want, or just keep the default name.
4. Click `Create Cluster`, then pick a username and a strong password, then click `Create user`.
5. For the `IP Address`, enter `0.0.0.0/0` -> `Add Entry`. Then, click `Finish and close`.
6. Click `Go to database` -> click `Connect`. Select `Python` for the driver and `3.6 or later` for version. Copy the connection string to a note file, then close the window.

![mongodb_con_str](https://i.imgur.com/9IwxYFg.png)

</details>

---

<details>
  <summary>Step 4: Connecting the cloud storage to label-studio</summary>
  
### Step 4: Connecting the cloud storage to label-studio

1. Go back to your label-studio application. Click on `Create project`.
2. Pick a name for your project, then click on `labeling setup` and select `object detection with bounding boxes`.
3. Remove the two default labels, then add the labels that you expect to see in your dataset (you can edit this later to add more). Make sure to add one label per line (note: the label should **not** include a backslash `\`!). Click on `Add`, then `save`.
4. Go the project settings (top right) -> click on `cloud storage` -> `add source storage`.
5. Under `Bucket Name` and `Region Name` fields, paste your bucket and region name from earlier in Step 2.

![bucket_info](https://i.imgur.com/VQJg5Tv.jpeg)

6. Under `S3 endpoint` field, copy the Service URL from this page (https://wasabi-support.zendesk.com/hc/en-us/articles/360015106031-What-are-the-service-URLs-for-Wasabi-s-different-storage-regions-) that correspond to your bucket's region, and paste it under `S3 endpoint`. For example, For `us-east-1`, the endpoint is `https://s3.wasabisys.com` (make sure to add `https://` at the start of the endpoint URL!).
7. Generate a access key id and secret by visiting Wasabi's console page (https://console.wasabisys.com) -> click on the key icon on the left -> click on `CREATE NEW ACCESS KEY`. Leave the default selection as it is, an click `create`.

![Generate key](https://i.imgur.com/D9Gggvu.jpeg)

9. Copy the keys to clipboard, then paste the value of the access key and secret key under `Access Key ID` and `Secret Key` in label-studio, respectively (clear the fields first). Clear the `Session token` field and leave it empty.
11. Toggle `Treat every bucket object as a source file` and `Recursive scan` to turn them ON, the click `Add storage`.

Now anything you upload the bucket can be synced to label studio!


**The upload interface:**

![upload](https://i.imgur.com/u4YwBKj.jpeg)

**After upload, sync:**

![sync](https://i.imgur.com/AZ7UKoy.jpeg)

Now you can see the images you uploaded as tasks in your label-studio project. You can label the objects in the image by opening a task -> clicking the label, then drawing a bounding box around the object -> submit.

</details>

---

## Model training and prediction (Step 5 to 6)

<details>
  <summary>Step 5: Set up the model prediction-workflow</summary>
 
### Step 5: Set up the model prediction-workflow

1. If you don't have an account yet, sign up on GitHub [here](https://github.com/signup) and verify your email.
2. First, fork the model repository by click on this button: [![](https://img.shields.io/badge/Fork-282a36?logo=github&style=for-the-badge)](https://github.com/bird-feeder/BirdFSD-YOLOv5/fork), then click `Create fork`.
3. In the fork page (the page that you will see after you clicked `create fork`). Click on your forked repository `settings` -> `Secrets`.

<img src="https://i.imgur.com/xlVfoxX.png"  width="720"> 

4. For every name in the table below, copy and paste the name to the `Name` field and use the value that correspond to that name for the secret's `Value` field. Repeat this for every secret listed below.

<img src="https://i.imgur.com/fOKMgHy.png"  width="720"> 


| Name                 | Value                                                                                                                                               |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| DB_CONNECTION_STRING | Use the connection string you copied in Step 3 for this secret's value                                                                              |
| DB_NAME              | label_studio                                                                                                                                        |
| LS_HOST              | Use the URL of your label-studio app for this secret's value (for example: https://my-labelstudio.herokuapp.com) – make sure you include `https://` |
| S3_ACCESS_KEY        | Use the value of `Access Key ID` you used in Step 4 for this secret's value                                                                         |
| S3_SECRET_KEY        | Use the value of `Secret Key` you used in Step 4 for this secret's value                                                                               |
| S3_REGION            | us-east-1                                                                                                                                           |
| S3_ENDPOINT          | https://s3.wasabisys.com                                                                                                                            |
| TOKEN                | Go to your label-studio app, click on your initials icon (top right) -> `Account & Settings`, then use the value of `Access Token` for this secret's value                   |

</details>

