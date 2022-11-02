#Defining the skeleton mern app

Home page: A view that renders at the root URL to welcome users to the web application.

Sign-up page: A view with a form for user sign-up, allowing new users to create a user account and redirecting them to a

sign-in page when successfully created.

Sign-in page: A view with a sign-in form that allows existing users to sign in so they have access to protected views and actions.

User list page: A view that fetches and shows a list of all the users in the database, and also links to individual user profiles.

Profile page: A component that fetches and displays an individual user's information. This is only accessible by signed-in users and also contains edit and delete options, which are only visible if the signed-in user is looking at their own profile.

Edit profile page: A form that fetches the user's information to prefill the form fields. This allows the user to edit the information and this form is accessible only if the logged-in user is trying to edit their own profile.

Delete user component: An option that allows the signed-in user to delete their own profile after confirming their intent

Menu navigation bar: A component that lists all the available and relevant views to the user, and also helps to indicate the user's current location in the application.

#Folder and file structure
.
├── Skeleton-MERN
├── client /
│ ├── assets/
│ │ └── images
│ ├── auth/
│ │ ├── api-auth.js
│ │ ├── auth-helper.js
│ │ ├── PriavteRoute.js
│ │ └── Signin.js
│ ├── core/
│ │ ├── Home.js
│ │ └── Menu.js
│ ├── user/
│ │ ├── api-user.js
│ │ ├── DeleteUser.js
│ │ ├── EditProfile.js
│ │ ├── Profile.js
│ │ ├── Signup.js
│ │ └── Users.js
│ ├── App.js
│ ├── main.js
│ ├── MainRouter.js
│ └── theme.js
├── config/
│ └── config.js
├── dist/
│ ├── bundle.js
│ └── server.generated.js
├── server/
│ ├── controller/
│ │ ├── auth.controller.js
│ │ └── user.contoller.js
│ ├── helpers/
│ │ └── dbErrorHandler.js
│ ├── models/
│ │ └── user.model.js
│ ├── routes/
│ │ ├── auth.routes.js
│ │ └── user.routes.js
│ ├── devBundle.js
│ ├── express.js
│ └── server.js
├── webpack.config.client.js
├── webpack.config.client.production.js
└── webpack.config.server.js

#Setting up for React development

Configuring Babel and Webpack

#Babel

yarn add --dev @babel/preset-react

Then, update .babelrc with the following code. This will include the module and also configure the react-hot-loader Babel plugin as required for the react-hotloader module.
mern-skeleton/.babelrc: {
"presets": [
["@babel/preset-env", {
"targets": { "node": "current" } } ], "@babel/preset-react" ],
"plugins": [ "react-hot-loader/babel" ] }
To put this updated Babel configuration to use, we need to update the Webpack configuration,

#Webpack
To bundle client-side code after compiling it with Babel, and also to enable reacthot-loader for faster development, install the following modules by running these commands from the command line:
yarn add -dev webpack-dev-middleware webpack-hot-middleware fileloader
yarn add react-hot-loader @hot-loader/react-dom
Then, to configure Webpack for frontend development and to build the production bundle, we will add a webpack.config.client.js file and a webpack.config.client.production.js file with the same configuration code we described in Chapter 2, Preparing the Development Environment.
With Webpack configured and ready for bundling the frontend React code, next, we will add some code that we can use in our development flow. This will make the fullstack development process seamless.

#Loading Webpack middleware for development

During development, when we run the server, the Express app should also load the Webpack middleware that's relevant to the frontend with respect to the configuration that's been set for the client-side code, so that the frontend and backend development workflow is integrated. To enable this, we will use the devBundle.js file. in order to set up a compile method that takes the Express app and configures it to use the Webpack middleware. The devBundle.js file in the server folder will look as follows.

mern-skeleton/server/devBundle.js:
import config from './../config/config' import webpack from 'webpack'
import webpackMiddleware from 'webpack-dev-middleware' import webpackHotMiddleware from 'webpack-hot-middleware'
import webpackConfig from './../webpack.config.client.js'
const compile = (app) => { if(config.env === "development"){ const compiler = webpack(webpackConfig) const middleware = webpackMiddleware(compiler, { publicPath: webpackConfig.output.publicPath
}) app.use(middleware) app.use(webpackHotMiddleware(compiler)) } }
export default { compile
}

In this method, the Webpack middleware uses the values set in webpack.config.client.js, and we enable hot reloading from the server-side using Webpack Hot Middleware.

Finally, we need to import and call this compile method in express.js by adding the following highlighted lines, but only during development.

mern-skeleton/server/express.js:
import devBundle from './devBundle' const app = express() devBundle.compile(app)

These two highlighted lines are only meant for development mode and should be commented out when building the code for production. When the Express app runs in development mode, adding this code will import the middleware, along with the client-side Webpack configuration. Then, it will initiate Webpack to compile and bundle the client-side code and also enable hot reloading.

The bundled code will be placed in the dist folder. This code will be needed to render the views. Next, we will configure the Express server app so that it serves the static files from this dist folder. This will ensure that the bundled React code can be loaded in the browser.

#Serving static files with Express

express.js.
mern-skeleton/server/express.js: import path from 'path'
const CURRENT_WORKING_DIR = process.cwd()
app.use('/dist', express.static(path.join(CURRENT_WORKING_DIR, 'dist')))

With this configuration in place, when the Express app receives a request at a route starting with /dist, it will know to look for the requested static resource in the dist folder before returning the resource in the response. Now, we can load the bundled files from the dist folder in the frontend.

#Updating the template to load a bundled script

mern-skeleton/template.js: ...

<body> <div id="root"></div> <script type="text/javascript" src="/dist/bundle.js"></script> </body>

#Adding React dependencies
Core React modules: react and react-dom
React Router modules: react-router and react-router-dom
Material-UI modules: @material-ui/core and @material-ui/icons
