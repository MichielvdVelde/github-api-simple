# Simple GitHub API wrapper for node.js

Lots of times you don't need a full-fledged GitHub API client with authentication and what-not. Maybe you just want to load public data, or index and process your public repositories somehow. This simple GitHub API 'wrapper' provides the basics and can access all non-restricted API end points. And it's easily extendable too!

The wrapper loads a JSON file (`routes.json`) which contains a name space, a method name and an end point URI. Together, all methods are dynamically created at startup-time. This makes it very easy to add new end points in the future. The library makes use of promises through [request-promise](https://github.com/request/request-promise).

Currently _not all suitable end points_ have been added. You can either wait for me to add them to `routes.json`, or you can do it yourself (and submit a pull request, of course! ;)).

## Install

```
npm install github-api-simple
```

## Usage

```js
const SimpleApi = require('github-api-simple');
let api = new SimpleApi(); // takes an options argument, see below

simpleApi.Repositories.getReposForUser('MichielvdVelde')
	.then(function(repos) {
		console.log('This user has %d repos', repos.length);
	});
```

The constructor takes an options argument, which is fed to [request-promise](https://github.com/request/request-promise). The defaults are:

```js
const DEFAULT_OPTIONS = {
	'baseUrl': 'https://api.github.com',
	'headers': {
		'User-Agent': 'github-api-simple v' + pkg.version + ' [nodejs] [https://github.com/MichielvdVelde/github-api-simple]'
	},
	'json': true
};
```

## Routes

### Users

#### Single user

All route methods take an optional `options` object as the last argument. This object is `extend()`-ed with the options you gave when usig `new`. For example, if you want to get all your repositories sorted by when it was last pushed to, you can:

```js
let options = {
	'qs': {
		'sort': 'pushed',
		'direction': 'desc' // optional, GitHub API uses 'desc' by default for 'pushed'
	}
}
simpleApi.Users.getUser('MichielvdVelde', options)
	.then(function(repos) {
		console.log('This user has %d repos', repos.length);
	});
```

[GitHub API reference](https://developer.github.com/v3/users/#get-a-single-user)

```js
simpleApi.Users.getUser('MichielvdVelde')
	.then(function(repos) {
		console.log('This user has %d repos', repos.length);
	});
```

#### All users

[GitHub API reference](https://developer.github.com/v3/users/#get-all-users)

```js
simpleApi.Users.getUsers('MichielvdVelde')
	.then(function(repos) {
		console.log('This user has %d repos', repos.length);
	});
```

### Repositories

#### Repositories for a single user

[GitHub API reference](https://developer.github.com/v3/repos/#list-user-repositories)

```js
simpleApi.Repositories.getReposForUser('MichielvdVelde')
	.then(function(repos) {
		console.log('This user has %d repos', repos.length);
	});
```

#### Single repository

[GitHub API reference](https://developer.github.com/v3/repos/#get)

```js
simpleApi.Repositories.getRepo('MichielvdVelde', 'github-api-simple')
	.then(function(repo) {
		console.log('This repo has %d watchers', repo.watchers_count);
	});
```


#### Contributors

[GitHub API reference](https://developer.github.com/v3/repos/#list-contributors)

```js
simpleApi.Repositories.getRepoContributors('MichielvdVelde', 'github-api-simple')
	.then(function(contributors) {
		console.log('This repo has %d contributors', contributors.length);
	});
```


#### Languages

[GitHub API reference](https://developer.github.com/v3/repos/#list-languages)

```js
simpleApi.Repositories.getRepoLanguages('MichielvdVelde', 'github-api-simple')
	.then(function(languages) {
		for(let language of languages) {
			console.log('language %s, %d bytes', language, languages[language]);
		}
	});
```


#### Teams

[GitHub API reference](https://developer.github.com/v3/repos/#list-teams)

```js
simpleApi.Repositories.getRepoTeams('MichielvdVelde', 'github-api-simple')
	.then(function(teams) {
		if(teams) {
			console.log('This repo has %d teams', teams.length);
		}
	});
```


#### Tags

[GitHub API reference](https://developer.github.com/v3/repos/#list-tags)

```js
simpleApi.Repositories.getRepoTags('MichielvdVelde', 'github-api-simple')
	.then(function(tags) {
		console.log('This repo has %d tags', tags.length);
	});
```


#### Branches

[GitHub API reference](https://developer.github.com/v3/repos/#list-branches)

```js
simpleApi.Repositories.getRepoBranches('MichielvdVelde', 'github-api-simple')
	.then(function(branches) {
		console.log('This repo has %d branches', branches.length);
	});
```


#### Single branch

[GitHub API reference](https://developer.github.com/v3/repos/#get-branche)

```js
simpleApi.Repositories.getRepoBranche('MichielvdVelde', 'github-api-simple', 'master')
	.then(function(branche) {
		console.log('Latest commit on master is %s', branche.commit.sha);
	});
```

# To do

* Add the rest of the applicable API end points to the routes file
* Add paging support
* MAke rate limiting headers available
* Write some tests

# Changelog

* 0.0.1 - 0.0.3 - 9 December 2015
  * (0.0.3) Improved readme
  * (0.0.2) Moved `routes.json` to dir `assets`
  * (0.0.2) Fixed stupid-ass path error
  * (0.0.1) Initial commit

## License

Copyright 2015 Michiel van der Velde.

This software is licensed under [the MIT License](LICENSE).
