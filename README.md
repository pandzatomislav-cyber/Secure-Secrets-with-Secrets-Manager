<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Secure Secrets with Secrets Manager

**Project Link:** [View Project](https://nextwork.ai/projects/d6a05ea4-6c31-5e53-8688-de54814d6209)

**Author:** Tomislav Pandza  
**Email:** pandza.tomislav@gmail.com

---

![Image](https://nextwork.ai/elated_teal_vibrant_raccoon/uploads/d6a05ea4-6c31-5e53-8688-de54814d6209_r7s8t9u0)

## Introducing Today's Project!

In this project, I will demonstrate how to use AWS Secrets Manager to securely store and retrieve secrets.
In step 1, we'll take a look at a simple web app that lists S3 buckets, clone it to our computer, and set it up to hardcode AWS credentials.
In step 2, we'll see what happens when we share this code (with the credentials hardcoded!) publicly on GitHub.
In step 3, we'll secure the credentials using AWS Secrets Manager.
In step 4, we'll update the web app code to use the credentials from Secrets Manager.
In step 5, we'll try share the code on GitHub again, and see how you can wipe your repository clean of all the hardcoded credentials!

### Tools and concepts

Services I used were IAM, Secrets Manager, S3, Git Hub and a windows terminal. Key concepts I learnt included how to fork a repository ans copy it on my local device, make changes to files and push them onto Git, create a secret and finally run the Web App with my own credentials for the secret mission.

### Project reflection

This project took me approximately 2 days as I had made mistakes and had to start from scratch. The most challenging part was getting the changes right on my local computer to push them onto Git. It was most rewarding to actually finish all of it and see how a real world scenario can be very tricky.

I chose to do this project today because I am moving towards cyber security and want to gain as much knowledge as possible.

## Hardcoding credentials

In this project, a sample web app is exposing the AWS access key id and the aws secret key. It is unsafe to harcode credentials because if we were to post this on GitHub or any other open source website, everyone woule be able to see our access credentials and could seriously hurt our firm by messing up our resources.

I've set up the initial configuration with fake AWS credentials. These credentials are just examples and won't make our web app work because for it to work, we would have to actually use the real credentials from an AWS account. We did this to show an example of how the credentials can be stolen when embeded in code.

![Image](https://nextwork.ai/elated_teal_vibrant_raccoon/uploads/d6a05ea4-6c31-5e53-8688-de54814d6209_j2k3l4m5)

## Using my own AWS credentials

As an extension for this project, I also decided to install uvicorn, fastapi and boto3 to set up my virtual environment. I installed them because we needed all of them in requirements.txt to be able to run my API

When I first ran the app, I ran into an error because this error means the AWS Access Key ID provided for the web app is not valid. Our app is running, but it can't access your AWS account because it doesn't have valid credentials! This is because we are using the placeholder credentials in config.py, which are not real AWS credentials.

To resolve the 'InvalidAccessKeyId' error, I updated the config.py to actually have real Access Key ID and Secret Access Key I made in AWS under IAM.

![Image](https://nextwork.ai/elated_teal_vibrant_raccoon/uploads/d6a05ea4-6c31-5e53-8688-de54814d6209_wghjteykut)

## Pushing Insecure Code to GitHub

Once I updated the web app code with credentials, I forked the repository because we need to have a version of the web app with a code containing the credentials in the code. A fork is different from a clone where we are taking the original code from a repository and publishing it with our own changes, whatever they may be.

To connect my local repository to the forked repository, I used "git init" to reinitialised the existing repository. Then I used "git remote add origin <my-forked-repo-url>" to connect to my forked repository. After that I used "git add ." to stage all the changes made in my repository and "git commit -m" to snapshot my changes . Finally, "git push -u origin main". This command uploads my local commits to my "origin" remote. The -u and main tells Git to set make the main branch the upstream branch, so next time I push code, I can just run git push and it'll automatically know where to push the code.

GitHub blocked my push because GitHub automatically scans one's code for secrets and blocks the push if it detects any, like AWS credentials. This is a good security feature to prevent accidental exposure of secrets!

![Image](https://nextwork.ai/elated_teal_vibrant_raccoon/uploads/d6a05ea4-6c31-5e53-8688-de54814d6209_o2p3q4r5)

## Secrets Manager

Secrets Manager is a service that helps you securely store and manage secrets, such as database credentials, API keys, and other sensitive information. Think of it as a digital vault for your secrets. I'm using it to store the AWS Access Key ID and the Secret Access Key. 

Another feature in Secrets Manager is Secret Rotation, the process of automatically changing your secrets on a regular schedule. This is a security best practice because it reduces the risk of compromised credentials. If a secret is compromised, it will only be valid for a limited time before it's automatically rotated. Secrets Manager can automatically rotate secrets for databases and other services. You can also configure custom rotation for other types of secrets. It's useful for high-risk credentials like database passwords, privileged API keys, and service account credentials.

Secrets Manager provides sample code in various languages, like Java, Java Script, Python3, C#, Ruby, Go and Rust.  This is helpful because many applications require different languages for different functions.

![Image](https://nextwork.ai/elated_teal_vibrant_raccoon/uploads/d6a05ea4-6c31-5e53-8688-de54814d6209_h2i3j4k5)

## Updating the web app code

I updated the config.py file to retrieve the secret I created to access my Acces Key ID and Secret Access Key. The get_secret() function will the secret, in the region we set up the secret, creates the boto3 client for interacting with Secrets Manager service, try...except block handles any potential errors when retrieving the secret.

I also added code to config.py to retrieve the credentials from the secret and assign them to Access Key ID, Secret Access Key and the region. This is important because the secret is returned in JSON string hence why we imported json at the top of the code. We also used json.loads() to parse the secret into Python dicitionary

![Image](https://nextwork.ai/elated_teal_vibrant_raccoon/uploads/d6a05ea4-6c31-5e53-8688-de54814d6209_v0w1x2y3)

## Rebasing the repository

Git rebasing is essentially making changes to your commit history.  I used it to remove the commit that has the credentials in the code. This was necessary because without the dropped commit, we will not be able to push the new config.py onto our forked git repository.

A merge conflict occurred during rebasing because we've changed the same lines of code in different commits. In our case, the commit we removed had the hardcoded credentials, and the next commit we did also changed the config.py file. So Git's now confused - if you remove the commit with the hardcoded credentials, should it delete the change you made after that commit too? I resolved the merge conflict by we'll need to manually edit the conflicting files to resolve these conflicts.

Once the merge conflict was resolved, I verified my hardcoded credentials were removed by checking config.py on GitHub after pushing the Secrets Manager version.

![Image](https://nextwork.ai/elated_teal_vibrant_raccoon/uploads/d6a05ea4-6c31-5e53-8688-de54814d6209_t5u6v7w8)

---

*Built with [NextWork](https://nextwork.ai) - [View this project](https://nextwork.ai/projects/d6a05ea4-6c31-5e53-8688-de54814d6209)*
