# Soundscape TV and client Web Apps

The Soundscape Web app consist of both the mobile client and tv/speaker host application. It utilizes the Samsung Multiscreen API to provide a jukebox style song sharing with multiscreen capable TVs and Speakers. You can read more about the Soundscape here - [link](https://github.com/MultiScreenSDK/webapp-audioplayer/blob/master/soundscape.md).

The project utilizes ReactJS for components and BabelJS for ES6/React code compilation support.

## App URLs

The latest TV and mobile web apps builds are hosted on AWS S3 and available here:
- [TV](http://s3-us-west-1.amazonaws.com/dev-multiscreen-examples/examples/soundscape/tv/index.html)
- [Mobile](http://s3-us-west-1.amazonaws.com/dev-multiscreen-examples/examples/soundscape/mobile/index.html)


## Getting Started

To get started clone the repo and run the following command.

```bash
$ npm install
```


## Source Code Structure

- **src** (all uncompiled code and assets)
	- **common** (common scripts shared for mobile and tv apps)
	- **mobile/tv**
		- **index.html** (staticaly copied upon build)
		- **index.js** (used as the entry point for mapping script dependencies)
		- **fonts** (staticaly copied upon build)
		- **images** (staticaly copied upon build)
		- **js** (compiled by babel using the index.js for dependency graphing upon build ... js and jsx files)
		- **styles** (compiled by lessc upon build using styles.less for the entry point)


## Building and Development


Once you have cloned the repo do simply use `npm install` the module dependencies. The postinstall script will create an initial build. Gulp is used for all task including compiling scripts, styles, and assets.

### Gulp tasks

**Build the application**
```bash
$ gulp || gulp build
```

**Run the local test server**
```bash
$ gulp server (available at http://localhost:3000/(mobile|tv)
```

**Develop with livereload support**
Watches for src file changes and recompiles the needed files. This includes livereload support. You can install the livereload extension [here](https://chrome.google.com/webstore/detail/livereload/jnihajbhpnppcggbcgedagnkighmdlei?hl=en).
```bash
$ gulp watch (available at http://localhost:3000/(mobile|tv)
```

## Communication Protocol (Channel Events)
Refer to this [document](https://github.com/MultiScreenSDK/webapp-audioplayer/blob/master/soundscape.md) for the communication protocol between TV host app and the mobile client apps.
