# single-spa-mfes
single spa based angular micro-forntends

## Create MFE app with create-single-spa cli

```
moni@moni-pc MINGW64 /d/Workshop/poc/microfrontends/single-spa-mfes (init-mf-apps)
$ create-single-spa
? Directory for new project .
? Select type to generate single-spa application / parcel
? Which framework do you want to use? angular
? Project name (can use letters, numbers, dash or underscore) micro-navbar
? Would you like to add Angular routing? Yes
? Which stylesheet format would you like to use? CSS
CREATE micro-navbar/angular.json (2730 bytes)
CREATE micro-navbar/package.json (1043 bytes)
CREATE micro-navbar/README.md (1065 bytes)   
CREATE micro-navbar/tsconfig.json (901 bytes)
CREATE micro-navbar/.editorconfig (274 bytes)
CREATE micro-navbar/.gitignore (548 bytes)   
CREATE micro-navbar/tsconfig.app.json (263 bytes)
CREATE micro-navbar/tsconfig.spec.json (273 bytes)
CREATE micro-navbar/.vscode/extensions.json (130 bytes)
CREATE micro-navbar/.vscode/launch.json (474 bytes)
CREATE micro-navbar/.vscode/tasks.json (938 bytes)
CREATE micro-navbar/src/favicon.ico (948 bytes)
CREATE micro-navbar/src/index.html (297 bytes)
CREATE micro-navbar/src/main.ts (214 bytes)
CREATE micro-navbar/src/styles.css (80 bytes)
CREATE micro-navbar/src/assets/.gitkeep (0 bytes)
CREATE micro-navbar/src/app/app-routing.module.ts (245 bytes)
CREATE micro-navbar/src/app/app.module.ts (393 bytes)
CREATE micro-navbar/src/app/app.component.html (23115 bytes)
CREATE micro-navbar/src/app/app.component.spec.ts (1091 bytes)
CREATE micro-navbar/src/app/app.component.ts (216 bytes)
CREATE micro-navbar/src/app/app.component.css (0 bytes)
✔ Packages installed successfully.
    Directory is already under version control. Skipping initialization of git.
ℹ Using package manager: npm
✔ Found compatible package version: single-spa-angular@7.1.0.
✔ Package information loaded.

The package single-spa-angular@7.1.0 will be installed and executed.
Would you like to proceed? Yes
✔ Packages successfully installed.
? Does your application use Angular routing? Yes
? What port should your project run on? 9202
    Added 'single-spa' as a dependency
    Added 'single-spa-angular' as a dependency
    Added 'style-loader' as a dependency
    Added '@angular-builders/custom-webpack' as a dependency
    Generated 'main.single-spa.ts
    Generated 'single-spa-props.ts
    Generated asset-url.ts
    Generated extra-webpack.config.js
    Using @angular-builders/custom-webpack builder.
    Updated angular.json configuration
    @angular-builders/custom-webpack:browser
    Warning: Since routing is enabled, an additional manual
    configuration will be required, see https://single-spa.js.org/docs/ecosystem-angular/#configure-routes
CREATE extra-webpack.config.js (303 bytes)
CREATE src/main.single-spa.ts (932 bytes)
CREATE src/app/empty-route/empty-route.component.ts (143 bytes)
CREATE src/single-spa/asset-url.ts (502 bytes)
CREATE src/single-spa/single-spa-props.ts (333 bytes)
UPDATE package.json (1404 bytes)
UPDATE tsconfig.app.json (196 bytes)
UPDATE angular.json (3021 bytes)
For further routing setup, see https://single-spa.js.org/docs/ecosystem-angular#configure-routes
Project setup complete!
Steps to test your Angular single-spa application:
1. Run 'npm run serve:single-spa:micro-navbar'
2. Go to http://single-spa-playground.org/playground/instant-test?name=micro-navbar&url=%2F%2Flocalhost%3A4200%2Fmain.js&framework=angular to see it working!
```

## Apply patch with relative path of mfe app

```
git apply patches/create-single-spa-ng-app.patch -p2 --directory=micro-navbar/
```

## Update selector 'app-root' in app.component.ts, index.html and main.single-spa.ts files with unique name per mfe . Examples:
### app.component.ts :

```
...
@Component({
  selector: 'header-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
...
```

### Index.html :

```
...
<body>
  <header-root></header-root>
</body>
</html>
```
### main.single-spa.ts:

```
...
const lifecycles = singleSpaAngular({
  bootstrapFunction: singleSpaProps => {
    singleSpaPropsSubject.next(singleSpaProps);
    return platformBrowserDynamic(getSingleSpaExtraProviders()).bootstrapModule(AppModule);
  },
  template: '<header-root />',
  Router,
  NavigationStart,
  NgZone,
});
...
```

# Update in app routing module
```
const routes: Routes = [
  { path: '**', component: EmptyRouteComponent },
];
```

```
  providers: [
    { provide: APP_BASE_HREF, useValue: '/' },
  ],
```

