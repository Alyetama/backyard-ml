# DEMO

# Data Preparation

### Step 1: Deploy Label-Studio

1. Sign up on Heroku: https://signup.heroku.com/ and verify your email.
2. Click this button: [<img src="https://www.herokucdn.com/deploy/button.svg" height="30px">](https://heroku.com/deploy?template=https://github.com/heartexlabs/label-studio/tree/heroku-persistent-pg).
3. Pick any name for the app (e.g., `label-studio-0`).
4. Change `DISABLE_SIGNUP_WITHOUT_LINK` from `0` to `1`.
5. Enter a username and password for the default account that you will use for label-studio.
6. Click `deploy`!

When your `label-studio` app is deployed, you will see something that like this:

![ls_deploy_ok](https://i.imgur.com/X8NuIkk.png)

Click on `View`, then login with the username and password you entered earlier.

---

### Step 2: Store your data in the cloud

You would wanna keep your data in the cloud so it's easily moved between different apps. For that, you will create a **S**imple **S**torage **S**ervice (S3) bucket, where all your data will be kept.
There are a lot of options for S3-compatible object storage service, but here, we will use Wasabi.

1. Sign up on Wasabi: https://wasabi.com/sign-up/ and verify your email.
2. Open the console: https://console.wasabisys.com, then click on `Buckets` -> `CREATE BUCKET`.

![create_bucket](https://i.imgur.com/9Sxl8tg.png)

3. Pick up a name for your bucket (e.g., `data-0`), select a region (e.g., `us-east-1`), then click `Next`.
4. Keep the options as they are, then click `Next`, then click `CREATE BUCKET`.

That's it! You made an S3 bucket! Now leave the page open, because we will go back to it later to upload our data.

---

### Step 3: Connect the bucket to label-studio

