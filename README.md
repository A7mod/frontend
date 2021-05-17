P.S. -Project Description at the end of these "instructions"..


# Getting Started with Create React App

This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app).

## Available Scripts

In the project directory, you can run:

### `npm start`

Runs the app in the development mode.\
Open [http://localhost:3000](http://localhost:3000) to view it in the browser.

The page will reload if you make edits.\
You will also see any lint errors in the console.

### `npm test`

Launches the test runner in the interactive watch mode.\
See the section about [running tests](https://facebook.github.io/create-react-app/docs/running-tests) for more information.

### `npm run build`

Builds the app for production to the `build` folder.\
It correctly bundles React in production mode and optimizes the build for the best performance.

The build is minified and the filenames include the hashes.\
Your app is ready to be deployed!

See the section about [deployment](https://facebook.github.io/create-react-app/docs/deployment) for more information.

### `npm run eject`

**Note: this is a one-way operation. Once you `eject`, you can’t go back!**

If you aren’t satisfied with the build tool and configuration choices, you can `eject` at any time. This command will remove the single build dependency from your project.

Instead, it will copy all the configuration files and the transitive dependencies (webpack, Babel, ESLint, etc) right into your project so you have full control over them. All of the commands except `eject` will still work, but they will point to the copied scripts so you can tweak them. At this point you’re on your own.

You don’t have to ever use `eject`. The curated feature set is suitable for small and middle deployments, and you shouldn’t feel obligated to use this feature. However we understand that this tool wouldn’t be useful if you couldn’t customize it when you are ready for it.

## Learn More

You can learn more in the [Create React App documentation](https://facebook.github.io/create-react-app/docs/getting-started).

To learn React, check out the [React documentation](https://reactjs.org/).

### Code Splitting

This section has moved here: [https://facebook.github.io/create-react-app/docs/code-splitting](https://facebook.github.io/create-react-app/docs/code-splitting)

### Analyzing the Bundle Size

This section has moved here: [https://facebook.github.io/create-react-app/docs/analyzing-the-bundle-size](https://facebook.github.io/create-react-app/docs/analyzing-the-bundle-size)

### Making a Progressive Web App

This section has moved here: [https://facebook.github.io/create-react-app/docs/making-a-progressive-web-app](https://facebook.github.io/create-react-app/docs/making-a-progressive-web-app)

### Advanced Configuration

This section has moved here: [https://facebook.github.io/create-react-app/docs/advanced-configuration](https://facebook.github.io/create-react-app/docs/advanced-configuration)

### Deployment

This section has moved here: [https://facebook.github.io/create-react-app/docs/deployment](https://facebook.github.io/create-react-app/docs/deployment)

### `npm run build` fails to minify

This section has moved here: [https://facebook.github.io/create-react-app/docs/troubleshooting#npm-run-build-fails-to-minify](https://facebook.github.io/create-react-app/docs/troubleshooting#npm-run-build-fails-to-minify)


# Project : frontend

 Moving along in the course, here we come across techniques of 'Creating a Production-Grade Workflow' using Docker. 
 By far, we've downloaded nodejs and created a react app and generated the application, without using docker.
 
 The whole idea is to create a react frontend and wrap it up inside a docker container and try testing and later deploying it.
 
 To test we'll be using Travis CI and for production we will try deploying all this to AWS EBS, which we're told to be automatic. 
 
 This is a `3 part project` where there's a `Development` phase, a `Testing` phase and a `Production` phase. 
 
 
## Real-time

### Development Phase
 
 Beginning the development part of out 3 part project, there are two dockerfiles `Dockerfile.dev` for development and `Dockerfile` for our usual dockerfile.
 
 Also, `node_modules` folder has to be deleted, just to stop Docker container from redoing it since it contains the `RUN npm install` line? And it also cuts our memory  from 198 MB to a puny 1.6 MB. It's a win win.
 
 We are able to see the webpage on localhost:3000, the creation of Create React App. To tweak some changes, we obviosly made some changes in the `apps.js` file. 
 
 #### Here is the problem 
 To see the changes reflect in the localhost, we either have to rebuilt the image again, which is tiresome, or come up with a better approach.
 
 #### The solution = `Docker Volumes` 
  We haven't used docker volumes since it's a pain in the ass. !!! For reals. 
  To cut a potentially very long story short, we're setting up a "mapping" between a folder inside the docker container with a folder outside the docker container. 
  P.S. - This is all the bhasad.
  
  The syntax is : `docker run -p 3000:3000 -v/app/node_modules -v $(pwd):/app <image_id>`  
  
  Syntax for WSL2 users : `docker run -it -p 3000:3000 -v /app/node_modules -v ~/frontend:/app <image_id>` or 
                          `docker run -it -p 3000:3000 -v /app/node_modules -v /home/USER/frontend:/app <image_id>`
  
  here the portnumbers are same, but they can be different. No problem there.
  
 ### Bhasad in the frontend
  
  When adding volumes and volume bookmarking to the docker run command you see these errors in your terminal:

   "Failed to compile.

   EACCES: permission denied, mkdir '/app/node_modules/.cache'"
   
   Well, it was good that we have a well explained solution now : https://www.udemy.com/course/docker-and-kubernetes-the-complete-guide/learn/lecture/11436998#questions/14297316 
   
   The changes have been made in the Dockerfile.dev above. The explaination link above will tell the rest. 
   And at last, used `docker-compose` to automate this whole rufus. 

### Testing
   We run tests by building Dockerfile.dev & running `docker run -it <image-id> npm run test`  .
   This is a one time thing, if we edit `App.test.js` file in our `src`folder, we don't see the changes reflect in the terminal. 
    
   So for live updating tests, there are two option, and both are equally not very helpful, but we can't do much about it. 
   1. Use `docker exec` command: `docker exec -it <image-id> npm run test` which will do the work and is fairly good, but we need to copy the said image-id after running docker       build -f Dockerfile.dev and it's far from automated. We do get a great advantage here i.e., we get a command-line interface to interact with the running tests like -           re-running, p, w, quit etc. 
   2. Obviously adding an additional service in the `docker-compose` file. This will obviously automate the process from the first step, but there's that drawback of not being able to interact with the interface and rerunning tests. 
          
  #### Need for Nginx (for web server)
  <will explain> 
  
  For the nginx server, the main `Dockerfile` which we left in the Development section will be included. 
  This file will contain two sections:
      1. One for `Build Phase` &
       2. One for `Run Phase` 
  
  Build Phase includes <will explain> and Run Phase includes <will explain> .
 
 
 
 
