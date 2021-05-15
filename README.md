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
 
 
## Real-time
 
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
  
 ### Bhasad in the frontend
  
  When adding volumes and volume bookmarking to the docker run command you see these errors in your terminal:

   "Failed to compile.

   EACCES: permission denied, mkdir '/app/node_modules/.cache'"
 
 
 
 
