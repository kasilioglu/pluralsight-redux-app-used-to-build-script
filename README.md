# Root Files & Folders
- src folder: Projeye ait dosyalar, react components, express server vs.
- tools folder: Build and server related code goes here.
- .editorconfig : Standart editor ayarları.
- .gitignore : Standart ignore dosyası, buna dist folder da ekli.
- .eslintrc : Eslint ayarları.
- .npmrc : npm config file, TODO : save-exact ayarı niye var?
- .babelrc : Babel config dosyası, react için yapılandırıldı. TODO : biraz bak, misal js-dev-env projede sadece latest yazıyor
- webpack.config.dev.js : Dev ortamı webpack yapılandırması.
- webpack.config.prod.js : Production ortamı webpack yapılandırması.
- package.json : Dependency details are here https://github.com/coryhouse/pluralsight-redux-starter/blob/master/README.md

# Tools (build scripts)
 ### build.js
 npm build script runs this directly. This sets environment as prod and generates minified bundles using webpack.config.prod. Also renders results(errors and warning if any and an info message) to terminal output.

 ### buildHtml.js
 This script runs with prebuild npm script. Copies index.html to dist folder and adds styles.css to head meta tag (Uses cheerio.js for server side jquery usage).
 TODO : dev-env projesinde herşeyi webpack yapmıştı. İncele.

 ### distServer.js
 Production server, uses express and applies compression. This script runs with postbuild npm script so that we ensure dist folder is ready.

 ### srcServer.js
 Development server, uses webpack middleware and applies hot reloading.

 ### startMessage.js
 Runs on prestart, just renders build info to terminal output.

 ### testSetup.js
 Runs with npm test script, registers babel for tests.
 TODO : burada dev-env'e göre daha detaylı bir incele.

# Webpack
 TODO


# Source Files

### index.html 
Index page. Any other html will be inserted inside <div id="app">, also links to bundle.js which will be generated via webpack, css file will be inserted via buildHtml (with server side dom manipulation). As any other js file (apart from tests) this one also placed in bundle.js

### index.js
Application entry point defined in webpack entry configuration. This is the one renders components (due to route) inside <div id="app"> element. Also configures store and sets initial state.

### index.test.js
TODO

### routes.js
React router definitions. Uses App.js as base component to act as a layout for all routes.

### Api folder
Files here are just stubs and acting as a server api.

## Actions
### actionTypes.js
Constant type name definitions.

### ajaxStatusActions.js
Exports ajax related actions.

### authorActions.js
Uses mock api to get author data and dispatch it to reducer. Author load is called in index.js on load and set to store as initial state.
TODO : what is dispatcher function ?

### courseActions.js
Uses mock api to get and update course data and dispatch results to reducers. 

## Reducers
 The concept of a reducer is that it takes the current state and an action and it returns the next state. It's a pure function that does not modify the current state.
 
### index.js
Root reducer definition, combines reducer functions into a single object behind the scenes. Its passed to store on store creation.


### initialState.js
Just initial state definitions such as empty lists. Its passed to store on creation and also used in reducers as default parameters.

### courseReducer.js, authorReducer.js, ajaxStatusReducer.js
Reducers used to update state. As state is immutable, a reducer creates an empty object, copies original state, updates data in newly created object and returs new object as new state. Uses Object.assign for this operation as its performant.

## Store
### configureStore.js
Creates redux store and called in index.js. Uses redux-immutable-state-invariant middleware to force immutability.Uses redux-thunk middleware TODO What for?

## Selectors
### selector.js
Returns a list of name-value pair objects for authors. Called in ManageCoursePage.js component and stored as local prop and than passed to CourseForm.js as prop to render authors in a select element.

## Components

### App.js
Acting as a layout component, defined in routes. Have loading prop managed by redux and passed to Header.js.

### AboutPage.js, HomePage.js
Just a presentation component.

## Common Components
Common components goes here, just like shared directory in mvc views. All presentation components.

### Header.js
To display navigation, has single prop 'loading' is used to display loading dots. Rendered inside App.js (layout) and loading prop state is managed there by redux.

### LoadingDots.js
To display ajax loading icon. Manages own state TODO neden redux yok

### SelectInput.js, TextInput.js
Those are just wrappers for regular html elements. Element properties are passed via props.

## Course Components

### CoursesPage.js
This container component lists courses via CourseList.js. Also has an action parameter to navigate course form.
Container components deals with data and passes to child (usually presentation component but may be some other container as well), here CoursesPage performs redux issues and passes data to presentation component CourseList, CoursesPage also renders some input to page this does not harms its container concept as that part is not something requires data or reusable. On the other hand, CourseList is a presentation component as its not dependent any logic and just renders data it accepts from any source.

### CourseList.js, CourseListRow.js
CourseList is presentation component that takes courses as prop and renders each course in a table. Each course is represented by a CourseListRow and this presentation component accepts a single course to display as a row.

### ManageCoursePage.js
A container component to manage course updates.

### CourseForm.js
A presentation component that accepts data and actions. This component is rendered inside ManageCoursePage.

# Redux flow on ajax calls to manage "loading" state
TODO

