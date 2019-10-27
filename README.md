`react-native-nowatch`
======================

Run `react-native` CLI without it starting a gazillion file watchers.

# Installation

	yarn add react-native-nowatch

# Use

Instead of calling `react-native` directly, call:

	./node_modules/.bin/react-native-nowatch

Or from npm scripts, simply:

	react-native-nowatch

# Why?

Ever had to run something like this?

	echo fs.inotify.max_user_instances=524288 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p
	echo fs.inotify.max_user_watches=524288   | sudo tee -a /etc/sysctl.conf && sudo sysctl -p
	echo fs.inotify.max_queued_events=524288  | sudo tee -a /etc/sysctl.conf && sudo sysctl -p

Ever seen an error like this?

	BUILD FAILED in 3m 0s

	error Failed to install the app. Make sure you have the Android development environment set up: https://facebook.github.io/react-native/docs/getting-started.html#android-development-environment. Run CLI with --verbose flag for more details.
	Error: Command failed: ./gradlew app:bundleReleaseJsAndAssets -PreactNativeDevServerPort=8081
	Picked up _JAVA_OPTIONS: -XX:+UnlockExperimentalVMOptions -XX:+UseCGroupMemoryLimitForHeap
	internal/fs/watchers.js:173
	    throw error;
	    ^

	Error: ENOSPC: System limit for number of file watchers reached, watch '/builds/example/kickass-app/node_modules/whatevrer'
	    at FSWatcher.start (internal/fs/watchers.js:165:26)
	    at Object.watch (fs.js:1258:11)
	    at NodeWatcher.watchdir (/builds/example/kickass-app/node_modules/sane/src/node_watcher.js:159:22)
	    at Walker.<anonymous> (/builds/example/kickass-app/node_modules/sane/src/common.js:109:31)
	    at Walker.emit (events.js:198:13)
	    at /builds/example/kickass-app/node_modules/walker/lib/walker.js:69:16
	    at FSReqWrap.args [as oncomplete] (fs.js:140:20)

	FAILURE: Build failed with an exception.

	* What went wrong:
	Execution failed for task ':app:bundleReleaseJsAndAssets'.
	> Process 'command 'node'' finished with non-zero exit value 1

	* Try:
	Run with --stacktrace option to get the stack trace. Run with --info or --debug option to get more log output. Run with --scan to get full insights.

	* Get more help at https://help.gradle.org

	BUILD FAILED in 3m 0s

	    at checkExecSyncError (child_process.js:629:11)
	    at execFileSync (child_process.js:647:13)
	    at runOnAllDevices (/builds/example/kickass-app/node_modules/@react-native-community/cli-platform-android/build/commands/runAndroid/runOnAllDevices.js:94:39)
	    at process._tickCallback (internal/process/next_tick.js:68:7)
	error Command failed with exit code 1.
	info Visit https://yarnpkg.com/en/docs/cli/run for documentation about this command.
	ERROR: Job failed: exit code 1

# Inspiration

[facebook/metro bug #355 - _Watch option is ignored when creating JestHasteMap_](https://github.com/facebook/metro/issues/355).
