# Cloudstream Extensions - Post DMCA guide ()

As many of you know, Cloudstream has been DMCA'd and extensions no longer work through the usual means. Specially repository URLs. Some kind users have taken up the mantle of uploading their own repository URLs, but it's possible those could get killed too. So it's important to know how to self-host these files.

### The usual process of adding a repository
1. Open the cloudstream app
2. Go to settings
3. Select "extensions" from the menu
4. Select "add repository"
5. Insert a repository name and URL. **IMPORTANT**: The url needs to point to a valid `repo.json` file, ex: `http://somesite.com/repo.json`

Step 5 is usually where things go wrong, because people either cannot find a working `repo.json` link, or the link works but no plugins are loaded. This is explained below.

### How do extensions work? And why is self-hosting difficult?

The cloudstream app by default does not ship with extensions by default to avoid copyright issues (or it tried to anyway). The way Cloudstream adds new sources (called "plugins") is by a 3 step process that requires 3 things:

* a `repo.json` file that's hosted somewhere (so it can be located via something like http://somesite.com/repo.json) which points to a `plugins.json` file
* a `plugins.json` file that's hosted somwhere which points to the `*.cs3` plugin files
* various files that end with `*.cs3` that need to be hosted somewhere, and are pointed at by `plugins.json`

Now, let's explain what these files look like. Repo.json looks like this:
```
{
  "name": "English providers repository",
  "description": "Cloudstream English Plugin Repository",
  "manifestVersion": 1,
  "pluginLists": [
    "https://somesitethatworks.com/plugins.json"
  ]
}
```

The important bit is the url under **pluginLists**. This needs to point to a a valid location of a `plugins.json` file which should look like this:

```
[
  {
    "iconUrl": "https://www.google.com/s2/favicons?domain=allanime.to&sz=%size%",
    "apiVersion": 1,
    "repositoryUrl": "https://github.com/recloudstream/cloudstream-extensions",
    "fileSize": 46813,
    "status": 1,
    "language": "en",
    "authors": [],
    "tvTypes": [
      "AnimeMovie",
      "Anime"
    ],
    "version": 9,
    "internalName": "AllAnimeProvider",
    "url": "https://raw.githubusercontent.com/recloudstream/cloudstream-extensions/builds/AllAnimeProvider.cs3",
    "name": "AllAnimeProvider"
  },
  { ... },
  { ... },
  { ... },
]
```

The important part here, is each row represents a plugin, and it needs to point to have valid urls for the `.cs3` files located under the "url" property as well as "repositoryUrl". These are the links that tend to break all the time. Finally, these `.cs3` files MUST be hosted somewhere safe, where this `plugin.json` file can point at them

### So how do we fix it?

We have 2 options: Either you find a working `repo.json` link that someone has configured alongside `plugins.json` and all the necessary `.cs3` files.

OR you can self host all these files yourself, and repair all the broken URL links. This is how we'd do it. This document should be included in the same folder where I dumped all the `.cs3` files for you to host wherever you want. For example, assuming you dumped all your `cs3` plugin files under a site like http://myfilehosting.com/someuser/cloudstreamextensions where a file could be accessed like so http://myfilehosting.com/someuser/cloudstreamextensions/someplugin.cs3, you'd need to host also a http://myfilehosting.com/someuser/cloudstreamextensions/repo.json file that looked like this:

```
{
  "name": "English providers repository",
  "description": "Cloudstream English Plugin Repository",
  "manifestVersion": 1,
  "pluginLists": [
    "http://myfilehosting.com/someuser/cloudstreamextensions/plugins.json"
  ]
}
```

As well as a http://myfilehosting.com/someuser/cloudstreamextensions/plugins.json file that looked like this

```
[
  {
    "iconUrl": "https://www.google.com/s2/favicons?domain=allanime.to&sz=%size%",
    "apiVersion": 1,
    "repositoryUrl": "http://myfilehosting.com/someuser/cloudstreamextensions",
    "fileSize": 46813,
    "status": 1,
    "language": "en",
    "authors": [],
    "tvTypes": [
      "AnimeMovie",
      "Anime"
    ],
    "version": 9,
    "internalName": "AllAnimeProvider",
    "url": "http://myfilehosting.com/someuser/cloudstreamextensions/AllAnimeProvider.cs3",
    "name": "AllAnimeProvider"
  },
  { ... },
  { ... },
  { ... },
]
```

As long as you correct the broken links in `plugins.json` and `repo.json`, and the files are hosted correctly, it should be smooth sailing.