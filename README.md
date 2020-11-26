# Gesture Collection - Streamlining project

The purpose of this project is to set up a continuous delivery pipeline for the application for future contributors and document the process. 

## Getting started
Clone this repository to begin working with it. You will need to set up Flutter for your environment. Here is a tutorial for configuration with VS Code:[VS Code setup](https://flutter.dev/docs/development/tools/vs-code])

## Running the Gesture Collection Flutter App

1) Navigate to the directory where you want to use for your development

2) Run git clone https://github.com/tmedeiros/gesture_collection.git to clone the repository

3) Run flutter doctor to make sure you have everything you need to run the app

4) Run flutter pub get

5) Run flutter run

6) Use the credential to connect to and administer Firebase:
    Mail Id: gesturedatacollection@gmail.com 
    Password: WearablesData

## CI/CD Tools
The build system used is Microsoft Azure DevOps. This is a cloud-hosted build system that clones the repository and builds it upon every push to the `master` branch (see GitHub contribution guidelines below).

The basic CI/CD flow of the pipeline looks like this:
1. Clone the `master` branch into the cloud-hosted build agent.
2. Install the Flutter environment onto the build agent.
3. Lint and build the app with the keys specified in the build's environment variables (these reference the debug.keystore to allow the app to be signed and deployed to devices).
4. Restore NuGet packages (dependencies required to run the tests).
5. Build the UI Tests (Xamarin.UITest)
6. Install a local Node 10 environment (required for the App Center Test task).
7. Push the UI tests and the built Flutter APK to App Center Test, where it is tested on one device. 

## Expanding Device Sets
To expand test coverage, add new tests to the UITest project. To expand the devices that are tested on, create a new Device Set within App Center Test. The device set is specified in the Azure DevOps pipeline configuration as well. 

### GitHub contribution guidelines
As such, commits to the `master` branch should be performed by the way of pull requests rather than direct commits from now on. A good way to accomplish this is the [Gitflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow#:~:text=Gitflow%20Workflow%20is%20a%20Git,designed%20around%20the%20project%20release.) workflow - in short, you have a `master` (or `main` in 2020) branch, which represents mainline releases, and a `develop` branch which is sort of the "working copy" of the code. 

New features branch off of `develop` into something like `login-screen-branch`, for example. When work is completed on your feature, merge it back into `develop`. When all the key features are complete in `develop`, create a pull request to merge it into `master`, which will in turn kick off the build. 

Do:
- clone the repo and work in it yourself
- create a `develop` branch off of master if one does not exist
- each developer should branch off of `develop` for new work

Don't:
- commit directly to `master`
- worry if you screw something up - there are lots of ways in git to revert changes!

## References
* [Flutter](https://flutter.dev/docs/development/tools/vs-code)
* [Xamarin.UITest](https://docs.microsoft.com/en-us/appcenter/test-cloud/frameworks/uitest/)
* [App Center Test](https://docs.microsoft.com/en-us/appcenter/test-cloud/)

## Gotchas
- App Center Build doesn't directly support Flutter applications. While this would be a lot easier, it invokes a Gradle wrapper that we cannot control, and it introduces conflicts. 
- The Azure DevOps pipeline is composed of tasks, and there are a few things that are not straightforward, like needing to restore NuGet packages before building the UITest project and setting the Node environment before invoking the App Center Test task. This experience will likely improve over time. 
- The App Center Build may possibly work with iOS out of the box with the pre-clone script supplied in the repository. See [this link](https://buildflutter.com/deploying-flutter-apps-via-appcenter/) for more info. 



