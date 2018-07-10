# Repo for reproducing issue [yarn#5978](https://github.com/yarnpkg/yarn/issues/5978)

## Summary of the issue

Yarn installs the hoisted version of a dependency (React 15) inside another dependency (react-redux) which doesn't explicitely requires it, only through peer dependencies. All while the app that requires react-redux, is using React 16.


## Detailed explanation

There are three independent packages. Two of those work with React 15, and the other works with React 16. The three of them work with react-redux v5.0.7.

When running `yarn install`, React 15 and react-redux v5.0.7 get hoisted at the root. The third package gets React 16 installed.
The third package also gets react-redux installed in its own `node_modules` because of "nohoist" config.

The issue is that **React 15 gets installed inside that no-hoisted version of react-redux**

```
$ tree -d .
.
├── applications
│   ├── my-first-app
│   │   └── frontend
│   ├── my-second-app
│   │   └── frontend
│   └── my-third-app
│       └── frontend
│           └── node_modules
│               ├── react@16.4.1
│               ├── react-dom@16.4.1
│               └── react-redux@5.0.7
│                   └── node_modules
│                       └── react@15.6.2
└── node_modules
    ├── react@15.6.2
    ├── react-dom@15.6.2
    ├── react-redux@5.0.7
    └── redux@3.7.2
    (...among others)
```

## Steps

Have install yarn v1.6.0 and then at the root run:

```
$ yarn install
```
