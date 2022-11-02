Create React App Build Status PRs Welcome
Logo

Create React apps with no build configuration.

Creating an App – How to create a new app.
User Guide – How to develop apps bootstrapped with Create React App.
Create React App works on macOS, Windows, and Linux.
If something doesn’t work, please file an issue.
If you have questions or need help, please ask in GitHub Discussions.

Quick Overview
npx create-react-app my-app
cd my-app
npm start
If you've previously installed create-react-app globally via npm install -g create-react-app, we recommend you uninstall the package using npm uninstall -g create-react-app or yarn global remove create-react-app to ensure that npx always uses the latest version.

(npx comes with npm 5.2+ and higher, see instructions for older npm versions)

Then open http://localhost:3000/ to see your app.
When you’re ready to deploy to production, create a minified bundle with npm run build.

npm start

Get Started Immediately
You don’t need to install or configure tools like webpack or Babel.
They are preconfigured and hidden so that you can focus on the code.

Create a project, and you’re good to go.

Creating an App
You’ll need to have Node 14.0.0 or later version on your local development machine (but it’s not required on the server). We recommend using the latest LTS version. You can use nvm (macOS/Linux) or nvm-windows to switch Node versions between different projects.

To create a new app, you may choose one of the following methods:

npx
npx create-react-app my-app
(npx is a package runner tool that comes with npm 5.2+ and higher, see instructions for older npm versions)

npm
npm init react-app my-app
npm init <initializer> is available in npm 6+

Yarn
yarn create react-app my-app
