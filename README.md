# The Movie Database Archiver

[![standard-readme compliant](https://img.shields.io/badge/readme%20style-standard-brightgreen.svg?style=flat-square)](https://github.com/RichardLitt/standard-readme)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](/docs/prs.md) [![License: GPL v3](https://img.shields.io/badge/License-GPL%20v3-blue.svg)](/docs/LICENSE.txt)

This is the repository of [filmArchiver.sh](/bin/filmArchiver.sh) - a tool for grabbing the day's latest posters from [The Movie Database](https://www.themoviedb.org/en) and posting them to [Arweave](https://www.arweave.org).

![](/images/tmdb.png)

## Table of Contents

- [Usage](#usage)
- [Example Output](#example-output)
- [Built Using](#built-using)  
- [Install](#install)
  - [Dependencies](#dependencies)
- [Maintainer](#maintainer)
- [Contributing](#contributing)
- [License](#license)

## Usage

Please fulfil the [dependencies](#dependencies) beforehand #tl;dr, you must be running [hooverd](https://github.com/samcamwilliams/hooverd), for which you'll need some [arweave tokens](https://tokens.arweave.org/). You'll also need an API key from [The Movie Database](https://www.themoviedb.org).

`filmArchiver.sh -k | --key YOUR_TMDB_API_KEY [-p | --prude] [-m | --movie-only]`

(If you do not want to include adult films, supply the `-p` argument. If you do not want video releases, supply the `-m` argument).

The final step of the `filmArchiver.sh` script (which you'll find in the `bin` subdirectory) uses [hooverd](https://github.com/samcamwilliams/hooverd) to push some generated html to [Arweave](https://www.arweave.org). The script will output a transaction key from [Arweave](https://www.arweave.org). It will look like this:

```
Transaction HtXA0tn_KdTTy9GgBrhMJ3yrYb4nRymkEgUNbKHTdA8 dispatched to arweave.net:443 with response: 200.
```

Once the transaction has been mined, you can load it in a browser. e.g, [https://arweave.net/HtXA0tn_KdTTy9GgBrhMJ3yrYb4nRymkEgUNbKHTdA8](https://arweave.net/HtXA0tn_KdTTy9GgBrhMJ3yrYb4nRymkEgUNbKHTdA8). That will display the day's international film releases (looks especially good on mobile). Enjoy!

To check the status of a transaction, e.g. `HtXA0tn_KdTTy9GgBrhMJ3yrYb4nRymkEgUNbKHTdA8`, load the following: [https://arweave.net/tx/HtXA0tn_KdTTy9GgBrhMJ3yrYb4nRymkEgUNbKHTdA8](https://arweave.net/tx/HtXA0tn_KdTTy9GgBrhMJ3yrYb4nRymkEgUNbKHTdA8) - if that had not yet been mined, it would've returned `Pending` (it can take up to 10 minutes to mine [Arweave](https://www.arweave.org) transactions).

Because the script finds the day's film releases, a good way of running [filmArchiver.sh](/bin/filmArchiver.sh) is via [cron](https://help.ubuntu.com/community/CronHowto):

`59 23    * * *   /yourFilmArchiverRepos/binfilmArchiver.sh -k YOUR_TMDB_API_KEY >> /some/log/file 2>&1`

That will run the script daily at 23:59. It will output the required transaction key to `/some/log/file`. You can then use that key to load the html (as above).

## Example Output

- [International Film Releases for Fri 15 Nov 17:42:31 GMT 2019](https://arweave.net/4bucA18F6RzNBqMLrtcwNt8mw-CzCWCc64Xu-G9yB_I)
- [International Film Releases for Thu 14 Nov 17:40:44 GMT 2019](https://arweave.net/wvrSWtEwTWQnBvQyJZJ6nZnoEW2fFLciRTNUieC6QV8)
- [International Film Releases for Wed 13 Nov 19:49:23 GMT 2019](https://arweave.net/IGyCmWfEzaJWUcm2MBGrNAHlwR4bdP9NhrGtSLjZJBY)
- [International Film Releases for Tue 12 Nov 19:54:17 GMT 2019](https://arweave.net/JZhq7NbLYbt4_Qd8GH4RSfyjyykZszActQTxAR5IF7Q)
- [International Film Releases for Mon 11 Nov 19:04:20 GMT 2019](https://arweave.net/Bbl3VWHCP_N8_3HdAGky1QhU5XqJ-fS99lEHrqd50xY)
- [International Film Releases for Sun 10 Nov 19:49:02 GMT 2019](https://arweave.net/2ILrvG3tg7Og0zKE2i2XiaRgipN3fN1iVWb_reB8q2g)
- [International Film Releases for Sat 9 Nov 13:52:50 GMT 2019](https://arweave.net/HtXA0tn_KdTTy9GgBrhMJ3yrYb4nRymkEgUNbKHTdA8)

## Built Using

- [Arweave](https://www.arweave.org)
- [The Movie Database](https://www.themoviedb.org)
- [hooverd](https://github.com/samcamwilliams/hooverd)
- [jq](https://stedolan.github.io/jq/)

## Install

Clone this repository and install all [dependencies](#dependencies).

### Dependencies

- The script runs on flavours of Linux (it will probably work on MacOS, too, but that has not been tested)
- [node](https://nodejs.org/en/)
- [npm](https://www.npmjs.com/)
- [hooverd](https://github.com/samcamwilliams/hooverd)
- [jq](https://stedolan.github.io/jq/)

You will need to create an account on [The Movie Database](https://www.themoviedb.org) and get one of their API keys (you supply that key to [filmArchiver.sh](/bin/filmArchiver.sh) via the `-k` argument - see [usage](#usage)). You will also need to have some [arweave tokens](https://tokens.arweave.org/).

Your [arweave tokens](https://tokens.arweave.org/) will come in an _arweave keyfile_ that you supply to [hooverd](https://github.com/samcamwilliams/hooverd), which must be monitoring port _1908_. The easiest way to do that is to daemonise [hooverd](https://github.com/samcamwilliams/hooverd), using [pm2](https://github.com/Unitech/pm2). At the time of writing, [hooverd](https://github.com/samcamwilliams/hooverd) does not appear to be a public [npm](https://www.npmjs.com/) package, so first clone the [hooverd](https://github.com/samcamwilliams/hooverd) repository. Then, put your _arweave keyfile_ (e.g _arweave-keyfile-oJViU9iJRPS-TcFmvVyJhxD5EBqErtMtgXfDdf9UWY4.json_) in the home directory of your cloned [hooverd](https://github.com/samcamwilliams/hooverd) repository and amend the _scripts_ section of [hooverd's](https://github.com/samcamwilliams/hooverd) `package.json` to include the following:

```
"start": "node hooverd --wallet-file ./YOUR_ARWEAVE_KEYFILE"
```

You will need to install [hooverd's](https://github.com/samcamwilliams/hooverd) node dependencies:

```
npm install
```

Then daemonise [hooverd](https://github.com/samcamwilliams/hooverd):

```
pm2 start "npm run start"
```

You can then run [filmArchiver.sh](/bin/filmArchiver.sh) as per [usage](#usage) instructions.

## Maintainer

[Steve Huckle](https://glowkeeper.github.io/) - created as part of a [gitcoin bounty](https://gitcoin.co/issue/ArweaveTeam/Bounties/15/3647).

## Contributing

Contributions welcome - please email [Steve Huckle](https://glowkeeper.github.io/).

## License

GNU General Public License v3.0

Please refer to the file: [LICENSE](/docs/LICENSE.txt) for the full text.
