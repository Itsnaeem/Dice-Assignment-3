# Dice-Assignment-3
This Assignment based CI CD GIT HUB Actions.

Create a Github repository for a simple web application using any web framework of your
choice.
Create GH actions workflow. Your workflow must run on every push to the main branch
and deploy the application to a cloud service (e.g., Heroku, Azure, AWS, or GCP). The
workflow should have following jobs,
○ Build: for building the application
○ Test: for testing the application (Run unit test)
○ Deploy: for deploying the application to a cloud service
Your workflow should use environment variables to store sensitive data, such as API
keys or database credentials.
Use public GitHub Actions runners.


###### In this Assignment, I use a Python APP which call `Hello World`.

I used the docker image & docker-compose to create the CICD Pipeline using GitHub Actions

The structure of my Python App is:

![image](https://github.com/Itsnaeem/Dice-Assignment-3/assets/46102040/6824c072-bb45-44dd-821a-c763f70dffad)

The content of **APP.py**

```
from flask import Flask
import unittest

app = Flask(__name__)

@app.route('/')
def hello():
    return "Hello, World!"

class TestApp(unittest.TestCase):
    def setUp(self):
        self.tester = app.test_client(self)

    def test_hello(self):
        response = self.tester.get('/')
        self.assertEqual(response.status_code, 200)
        self.assertEqual(response.data, b'Hello, World!')

if __name__ == '__main__':
    # If the script is run directly, start the Flask app
    app.run(debug=True, host='0.0.0.0', port=5000)
else:
    # If the script is imported, run the tests
    unittest.main()
```

in **requirements.txt**
```
Flask
```

Then I create the **Dockerfile** of my project.

```
# Use the official Python image as a base image
FROM python:3.8

# Set the working directory inside the container
WORKDIR /app

# Copy the application code into the container
COPY . /app

# Install dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Expose the port on which the application will run
EXPOSE 5000

# Command to run the application
CMD ["python", "app.py"]
```

Using this I first run the project in my `localhost:5000`
it works.

using this command **docker build -t app/app1:latest .**

The output is:

```
docker build -t app/app1:latest .
[+] Building 143.2s (9/9) FINISHED                                                                                                                                                                    docker:desktop-linux
 => [internal] load build definition from Dockerfile                                                                                                                                                                  0.0s
 => => transferring dockerfile: 425B                                                                                                                                                                                  0.0s
 => [internal] load .dockerignore                                                                                                                                                                                     0.0s
 => => transferring context: 2B                                                                                                                                                                                       0.0s
 => [internal] load metadata for docker.io/library/python:3.8                                                                                                                                                         3.5s
 => [1/4] FROM docker.io/library/python:3.8@sha256:587b52ae38b024094a979998915ca7c8ba4f81ac4a0ae246c8cfcfb7465cdadf                                                                                                 130.9s
 => => resolve docker.io/library/python:3.8@sha256:587b52ae38b024094a979998915ca7c8ba4f81ac4a0ae246c8cfcfb7465cdadf                                                                                                   0.3s
 => => sha256:d7ead716e969d3b61aea3c3d6db55a50afbc41f7562b825ebbb69cf011793b16 2.01kB / 2.01kB                                                                                                                        0.0s
 => => sha256:2cc8a79ea641f7fd11724ed7efed931bff5388907505a58b869dea50264cf257 7.38kB / 7.38kB                                                                                                                        0.0s
 => => sha256:587b52ae38b024094a979998915ca7c8ba4f81ac4a0ae246c8cfcfb7465cdadf 1.86kB / 1.86kB                                                                                                                        0.0s
 => => sha256:6a299ae9cfd996c1149a699d36cdaa76fa332c8e9d66d6678fa9a231d9ead04c 49.58MB / 49.58MB                                                                                                                     32.8s
 => => sha256:e08e8703b2fb5e50153f792f3192087d26970d262806b397049d61b9a14b3af5 24.05MB / 24.05MB                                                                                                                     24.9s
 => => sha256:68e92d11b04ec0fe48e60d59964704aca234084f87af5d1a068c49456b37fe3d 64.14MB / 64.14MB                                                                                                                     60.3s
 => => sha256:5b9fe7fef9befda786bc8e1dd1ae42ffd8b9c37a4cce3c433e65ebb890cb1672 211.11MB / 211.11MB                                                                                                                  109.1s
 => => extracting sha256:6a299ae9cfd996c1149a699d36cdaa76fa332c8e9d66d6678fa9a231d9ead04c                                                                                                                             2.0s
 => => sha256:09864a904dd0b3b594959a6dfd2aff6d693ecce79a3c37484bb2e0a95fbf51a7 6.39MB / 6.39MB                                                                                                                       40.9s
 => => extracting sha256:e08e8703b2fb5e50153f792f3192087d26970d262806b397049d61b9a14b3af5                                                                                                                             0.6s
 => => sha256:a1422769d5bee1f5fd141768ada4206e4684c58f26a1b7cf75cefb562a2542cb 17.28MB / 17.28MB                                                                                                                     52.5s
 => => sha256:fad1ae7101cde1a81903107b97c1c021897a9a29d144605b96357a93684e96f0 244B / 244B                                                                                                                           53.2s
 => => sha256:455fe649c3e1774e2a3defc97abed480b858c7a4787bfb90e077e799c0408caf 2.85MB / 2.85MB                                                                                                                       54.4s
 => => extracting sha256:68e92d11b04ec0fe48e60d59964704aca234084f87af5d1a068c49456b37fe3d                                                                                                                             2.5s
 => => extracting sha256:5b9fe7fef9befda786bc8e1dd1ae42ffd8b9c37a4cce3c433e65ebb890cb1672                                                                                                                             9.1s
 => => extracting sha256:09864a904dd0b3b594959a6dfd2aff6d693ecce79a3c37484bb2e0a95fbf51a7                                                                                                                             0.3s
 => => extracting sha256:a1422769d5bee1f5fd141768ada4206e4684c58f26a1b7cf75cefb562a2542cb                                                                                                                             0.7s
 => => extracting sha256:fad1ae7101cde1a81903107b97c1c021897a9a29d144605b96357a93684e96f0                                                                                                                             0.0s
 => => extracting sha256:455fe649c3e1774e2a3defc97abed480b858c7a4787bfb90e077e799c0408caf                                                                                                                             0.2s
 => [internal] load build context                                                                                                                                                                                     1.3s
 => => transferring context: 8.02MB                                                                                                                                                                                   0.7s
 => [2/4] WORKDIR /app                                                                                                                                                                                                0.3s
 => [3/4] COPY . /app                                                                                                                                                                                                 0.7s
 => [4/4] RUN pip install --no-cache-dir -r requirements.txt                                                                                                                                                          7.2s
 => exporting to image                                                                                                                                                                                                0.5s
 => => exporting layers                                                                                                                                                                                               0.5s
 => => writing image sha256:6c3bc2c710494caf585f8557566ebed56edfb77e4c9b717212c08330e7e2fb7c                                                                                                                          0.0s
 => => naming to docker.io/app/app1:latest                                                                                                                                                                            0.0s

View build details: docker-desktop://dashboard/build/desktop-linux/desktop-linux/5zwixs8bczlsb0zwo6svcvlip

What's Next?
  1. Sign in to your Docker account → docker login
  2. View a summary of image vulnerabilities and recommendations → docker scout quickview
```

```bash
docker run -p 5000:5000 e35b0fac507d
```
![image](https://github.com/Itsnaeem/Dice-Assignment-3/assets/46102040/497bbcc2-5d70-4b8a-8a26-d696c44c294a)


Browser Output:

![image](https://github.com/Itsnaeem/Dice-Assignment-3/assets/46102040/c2bdf463-22f5-4c03-bae9-cfa60e8b21e3)

Then the Next part is to create AWS Ec2 Instance:


![image](https://github.com/Itsnaeem/Dice-Assignment-3/assets/46102040/55bb7647-dd41-4faa-885e-a1a7cefe0e7f)

in security I opened the Port 5000, 80, 22, 443

![image](https://github.com/Itsnaeem/Dice-Assignment-3/assets/46102040/276e118f-4eb5-472c-b64b-a918c7a70ffc)

& then create the pipeline using **GitHub Actions**:

GitHub Project:

![image](https://github.com/Itsnaeem/Dice-Assignment-3/assets/46102040/7d1192c6-1aea-4159-8aec-219e900be268)

Click on GitHub Actions, Create workflows for this project.


```bash
name: Deploy to AWS EC2

on:
  push:
    branches:
      - main
jobs:
    deploy:
      runs-on: ubuntu-latest
      env:
        VERSION: "1.0.0"
        PUBLICIP: "51.20.255.121"
        PROJECT_NAME: "/home/ubuntu/Dice-Assignment-3"
        USERNAME: "ubuntu"
        PORT: "22"

      steps:
        - name: Checkout code
          uses: actions/checkout@v2

        - name: Configure SSH
          uses: webfactory/ssh-agent@v0.7.0
          with:
            ssh-private-key: ${{ secrets.SSH_PRIVATE_KEY }}

        - name: SSH into server and Pull branch
          run: |
            ssh -o StrictHostKeyChecking=no ${{ env.USERNAME }}@${{ env.PUBLICIP }} "cd ${{ env.PROJECT_NAME }} && git pull"

        - name: Run Docker Compose
          run: |
            ssh -o StrictHostKeyChecking=no ${{ env.USERNAME }}@${{ env.PUBLICIP }} "cd ${{ env.PROJECT_NAME }} && docker-compose down && docker-compose up -d --build"
# addcomment
```
![image](https://github.com/Itsnaeem/Dice-Assignment-3/assets/46102040/e7ce5a21-69ea-43a5-9d02-a67bb5129c99)

in this Yml, when I push the project on Github it will automatically deploy in AWS EC2.

in AWS EC2 I install the Packages of Docker, Docker Compose, Python3, git.

Then git clone of my project in AWS EC2.

The path is 
```
/home/ubuntu/Dice-Assignment-3
```

So in local, **git status**
```
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   Dockerfile

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	app.py

no changes added to commit (use "git add" and/or "git commit -a")
```
git add.
git commit -m
git push

now in GitHub Actions 

![image](https://github.com/Itsnaeem/Dice-Assignment-3/assets/46102040/878ca10b-90eb-4685-8ee2-b2ac2be67521)

click on deploy:
![image](https://github.com/Itsnaeem/Dice-Assignment-3/assets/46102040/43d656e2-b1fc-4a95-8420-b6adf7f07520)


the job is running
![image](https://github.com/Itsnaeem/Dice-Assignment-3/assets/46102040/6fe20c14-2770-4703-b87e-67a33db3436f)

![image](https://github.com/Itsnaeem/Dice-Assignment-3/assets/46102040/bbfd333a-202c-4470-b228-b3a6ba3d68d9)


All the Jobs completed:

![image](https://github.com/Itsnaeem/Dice-Assignment-3/assets/46102040/375f98f1-0102-4585-bebc-bc16ea2f0344)

Now I will change or add comments in any then check again.

![Screenshot from 2024-02-01 17-16-29](https://github.com/Itsnaeem/Dice-Assignment-3/assets/46102040/8444a38d-c3b0-46b6-9564-691d572c7bc2)

![Screenshot from 2024-02-01 17-19-55](https://github.com/Itsnaeem/Dice-Assignment-3/assets/46102040/8dd046b0-e4fa-40e2-b082-0f7442da1ea5)

The Job of Workflow is running.

![Screenshot from 2024-02-01 17-20-21](https://github.com/Itsnaeem/Dice-Assignment-3/assets/46102040/beb36661-1360-4a90-aba3-346771b9c36d)


![Screenshot from 2024-02-01 17-20-29](https://github.com/Itsnaeem/Dice-Assignment-3/assets/46102040/d0eb5f1e-5072-493f-943b-091959095057)


![Screenshot from 2024-02-01 17-20-36](https://github.com/Itsnaeem/Dice-Assignment-3/assets/46102040/7f5b17f6-4f9f-4b03-8dfa-be3f4ca3eeed)

& its also accessible in Browser using IP.

![Screenshot from 2024-02-01 17-20-56](https://github.com/Itsnaeem/Dice-Assignment-3/assets/46102040/6aa47cdb-e9e1-4105-aeca-33d7c0aab805)

#### END of Assignment 3












