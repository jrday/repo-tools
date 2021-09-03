# GitHub repo visibility and default branch wrangling commands

Fire up your favorite shell, and use `curl`, `xargs` and [`jq`](https://stedolan.github.io/jq/) plus a GitHub personal access token with repo scope. 

## Make a public repo private

```bash
curl \
	--user jrday-fc:$TOKEN \
	--data '{"private": "true"}' \
	--request PATCH https://api.github.com/repos/jrday-fc/secodont
```

## Make all public repos in a user account private

```bash
curl --request GET https://api.github.com/users/jrday-fc/repos | \
	jq --raw-output '.[] .name' |  \
 	xargs -I % \
		curl \
			--user jrday-fc:$TOKEN \
			--data '{"private": "true"}' \
    			--request PATCH https://api.github.com/repos/jrday-fc/%
```

## Rename default branch from `master` to `main` in a repo

```bash
curl \
	--user jrday-fc:$TOKEN \
 	--data '{"new_name":"main"}'
	--request POST https://api.github.com/repos/jrday-fc/secodont/branches/master/rename
```

## Rename default branch from `master` to `main` in all repos

```bash
curl \
	--user jrday-fc:$TOKEN \
	--request GET https://api.github.com/user/repos | \
		jq --raw-output '.[] .name' |  \
		xargs -I % \
			curl \
				--user jrday-fc:$TOKEN \
				--data '{"new_name":"main"}' \
				--request POST https://api.github.com/repos/jrday-fc/%/branches/master/rename
```
