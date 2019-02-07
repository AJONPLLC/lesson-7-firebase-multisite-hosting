
# AJonP - Resources
🎥 YouTube: https://bit.ly/ajonp-youtube-sub  
🌎 Site: https://ajonp.com  
📦 GitHub: https://github.com/ajonpllc  
🎓Lessons: https://ajonp.com/lessons/   
🗞AJ’s Week in Web: https://bit.ly/aj-week-in-web   
💬 Slack: https://bit.ly/ajonp-slack-invite   
🐦 Twitter: https://bit.ly/ajonp-twitter  

# Firebase Multisite Hosting (Multiple Sites)

This lesson will focus on a very easy solution to having multiple web projects hosted under the same domain. 

{{% blockquote-warning %}}

> Firebase limits a single domain for all of your projects. Once you have associated this domain you not be able to use it again for any other project. For example we would like to use lessons.ajonp.com but since we are using ajonp-ajonp-com for ajonp.com domain we are unable to use this.

{{% /blockquote-warning %}}

> We also wanted to mention that the video may not match exactly what is in the lesson7 repo, we are sorry about that, initially we were going to use two projects with a single domain. Please pull the repo directly and these steps should help you get to a finished setup quickly.

## Lesson Steps
1. Switching Firebase Plans
1. Create 4 Firebase Hosting Sites
1. Example Multiple Hugo Themes deploying to Firebase Multisite
1. Example Angular Project with Multiple projects deploying to Firebase Multisite
1. Google Cloud Builder - Trigger

# Firebase Pricing Plans

## New Project
If you have just created a new project you will need to upgrade in order to use Multisite hosting.

![Upgrade Plan](https://res.cloudinary.com/ajonp/image/upload/f_auto,fl_lossy,q_auto/v1545142662/ajonp-ajonp-com/7-lesson-firebase-multisite-hosting/Screen_Shot_2018-12-17_at_2.56.02_PM.png)

![Included Pricing](https://res.cloudinary.com/ajonp/image/upload/f_auto,fl_lossy,q_auto/v1545142662/ajonp-ajonp-com/7-lesson-firebase-multisite-hosting/Screen_Shot_2018-12-17_at_3.00.55_PM.png)

We recommend the Blaze plan as you will end up paying a lot less (if anything) for all of your small projects. If you go with the Flame plan you are guaranteed to pay $25.

![Firebase Plans](https://res.cloudinary.com/ajonp/image/upload/f_auto,fl_lossy,q_auto/v1545142662/ajonp-ajonp-com/7-lesson-firebase-multisite-hosting/Screen_Shot_2018-12-17_at_2.57.48_PM.png)

Hosting Free limit on the Blaze plan
![Free Included Items](https://res.cloudinary.com/ajonp/image/upload/f_auto,fl_lossy,q_auto/v1545142662/ajonp-ajonp-com/7-lesson-firebase-multisite-hosting/Screen_Shot_2018-12-17_at_3.01.55_PM.png)

# Create 4 Firebase Hosting Sites
You can name these whatever fits your project, if you using ajonp* you will probably have issues as we am already using those.

- [ajonp-lesson-7 (Hugo)](https://ajonp-lesson-7.firebaseapp.com/)
- [ajonp-lesson-7-admin (Angular)](https://ajonp-lesson-7-admin.firebaseapp.com )
- [ajonp-lesson-7-amp (Hugo)](https://ajonp-lesson-7-amp.firebaseapp.com )
- [ajonp-lesson-7-app (Angular)](https://ajonp-lesson-7-app.firebaseapp.com )

Example for your site:

- mysite
- mysite-admin
- mysite-amp
- mysite-app

![Example Site Creation](https://res.cloudinary.com/ajonp/image/upload/f_auto,fl_lossy,q_auto/v1545158121/ajonp-ajonp-com/7-lesson-firebase-multisite-hosting/axpst0mag0khcvqqlijc.png)
![All Sites](https://res.cloudinary.com/ajonp/image/upload/f_auto,fl_lossy,q_auto/v1545160404/ajonp-ajonp-com/7-lesson-firebase-multisite-hosting/aaqgcyvubuggrk8fsqqm.png)

# Example Multiple Hugo Themes deploying to Firebase Multisite

{{% blockquote-success %}}

> You don't need to install anything if you just deploy to Google Cloud. In this lesson reference [Cloud Build](https://ajonp.com/lessons/7-firebase-multisite-hosting/#cloud-build) Also see [https://ajonp.com/lessons/2-firebase-ci/](https://ajonp.com/lessons/2-firebase-ci/) for more details on CI/CD.

{{% /blockquote-success %}}

Once you clone [https://github.com/AJONPLLC/lesson-7-firebase-multisite-hosting](https://github.com/AJONPLLC/lesson-7-firebase-multisite-hosting) you can run it locally, but you must build out all of the sites before doing this. 


## Remove git reference
Please remove the reference to AJONPLLC, then you can add any git repo you would like.
```sh
git remote rm origin
```

## Add new reference
1. Create a new github site (or Gitlab or Google Cloud or...)
![Git Sample Repo](https://res.cloudinary.com/ajonp/image/upload/f_auto,fl_lossy,q_auto/v1545162597/ajonp-ajonp-com/7-lesson-firebase-multisite-hosting/lnstebijo3p2ijyn1jzw.png)

1. Then add the new repo back to your site using this command, replace **ajonp/a-sample-repo.git** with yours.
```sh
git remote add origin https://github.com/ajonp/a-sample-repo.git
git push -u origin master
```

## Steps for building hugo sites

1. Git Submodule commands, make sure you have [Git CLI](https://git-scm.com/downloads).
```sh
git submodule init
git submodule update --recursive --remote
```
This will update to the latest remote builds for ajonp.com, including both the content and themes that we host publicly

1. Hugo commands, make sure you have the [Hugo CLI](https://gohugo.io/getting-started/installing/) installed.
```sh
hugo -d dist/ajonp -v -t ajonp-hugo-ionic --config config.toml,production-config.toml  
hugo -d dist/ajonp-amp -v -t ajonp-hugo-amp --config config.toml,production-config.toml
```

# Example Angular Project with Multiple projects deploying to Firebase Multisite

## Steps for building Angular sites

1. NPM commands, make sure you have [Node Js](https://nodejs.org/en/download/) installed.
```sh
npm install
```

1. Angular commands, make sure you have the [Angular CLI](https://cli.angular.io/) installed.
```sh
ng build --prod --project ajonp-admin
ng build --prod --project ajonp-app
```

1. Serve locally using firebase, make sure you have [Firebase CLI](https://firebase.google.com/docs/cli/)
```sh
firebase serve
```

# Google Cloud Builder - Trigger
Please add a trigger to your repository reference [Firebase CI/CD lesson](https://ajonp.com/lessons/2-firebase-ci/) for more details.

## Cloud Build
Now after having tried all of the manual build steps out, you really don't need any of them. You can run all of this in the cloud by setting up the commit trigger from above.

cloudbuild.yaml
```yaml
steps:
# Pull in the required submodules for our themes and content
- name: gcr.io/cloud-builders/git
  args: ['submodule', 'init']
- name: gcr.io/cloud-builders/git
  args: ['submodule', 'update', '--recursive', '--remote']
# Install all of our dependencies
- name: 'gcr.io/cloud-builders/npm'
  args: ['install']
# Build the hugo image
- name: 'gcr.io/cloud-builders/docker'
  args: [ 'build', '-t', 'gcr.io/$PROJECT_ID/hugo', './dockerfiles/hugo' ]
# Build ajonp-hugo-ionic -> dist/ajonp
- name: 'gcr.io/$PROJECT_ID/hugo'
  args: [ 'hugo',"-d", "dist/ajonp", "-v", "-t", "ajonp-hugo-ionic", "--config", "config.toml,production-config.toml"]
# Build ajonp-hugo-amp -> dist/ajonp-amp
- name: 'gcr.io/$PROJECT_ID/hugo'
  args: [ 'hugo',"-d", "dist/ajonp-amp", "-v", "-t", "ajonp-hugo-amp", "--config", "config.toml,production-config.toml"]
# Build the hugo image
- name: 'gcr.io/cloud-builders/docker'
  args: [ 'build', '-t', 'gcr.io/$PROJECT_ID/ng:latest', './dockerfiles/ng' ]
# Build ajonp-admin -> dist/ajonp-admin
- name: 'gcr.io/$PROJECT_ID/ng:latest'
  args: ['build', '--prod', '--project', 'ajonp-admin']
# Build ajonp-admin -> dist/ajonp-app
- name: 'gcr.io/$PROJECT_ID/ng:latest'
  args: ['build', '--prod', '--project', 'ajonp-app']
# Build the firebase image
- name: 'gcr.io/cloud-builders/docker'
  args: [ 'build', '-t', 'gcr.io/$PROJECT_ID/firebase', './dockerfiles/firebase' ]
# Deploy to firebase
- name: 'gcr.io/$PROJECT_ID/firebase'
  args: ['deploy', '--token', '${_FIREBASE_TOKEN}']
# Optionally you can keep the build images
# images: ['gcr.io/$PROJECT_ID/hugo', 'gcr.io/$PROJECT_ID/firebase']
```

## Cloud Build locally
If you would like to debug locally please make sure you read [Building and debugging locally](https://cloud.google.com/cloud-build/docs/build-debug-locally).
This will require that you have [Docker](https://docs.docker.com/install/#server) installed locally.

### Install necessary components
Install docker-credential-gcr
```sh
gcloud components install docker-credential-gcr
```
Configure Docker
```sh
docker-credential-gcr configure-docker
```

Check to see if version is working
```sh
cloud-build-local --version
```

### Build Locally
In the root directory of your project, you can then run this command (**don't forget** the "." as this tells Cloudbuild to use the current directory)

```sh
cloud-build-local --config=cloudbuild.yaml --dryrun=false --substitutions _FIREBASE_TOKEN=EXAMPLE .
```

You will notice that this output matches what happens in the cloud when Google Cloud Build executes

Local Output
```sh
Starting Step #0
Step #0: Pulling image: gcr.io/cloud-builders/git
Step #0: Unable to find image 'gcr.io/cloud-builders/docker:latest' locally
Step #0: latest: Pulling from cloud-builders/docker
Step #0: 75f546e73d8b: Pulling fs layer
Step #0: 0f3bb76fc390: Pulling fs layer
Step #0: 3c2cba919283: Pulling fs layer
Step #0: 252104ea43c6: Pulling fs layer
Step #0: 252104ea43c6: Waiting
Step #0: 3c2cba919283: Verifying Checksum
Step #0: 3c2cba919283: Download complete
Step #0: 0f3bb76fc390: Verifying Checksum
Step #0: 0f3bb76fc390: Download complete
Step #0: 75f546e73d8b: Verifying Checksum
Step #0: 75f546e73d8b: Download complete
Step #0: 75f546e73d8b: Pull complete
Step #0: 0f3bb76fc390: Pull complete
Step #0: 3c2cba919283: Pull complete
```

Google Cloud Builder Output
![Google Cloud Builder Output ](https://res.cloudinary.com/ajonp/image/upload/f_auto,fl_lossy,q_auto/v1545169224/ajonp-ajonp-com/7-lesson-firebase-multisite-hosting/ln3docmhljhbkphqio8a.png)
