---
title: Mar 21
draft: false
tags:
  - NextJS
  - standalone
---
## 1. Amplify conditional environment injection
```yml
version: 1 
frontend: 
	phases: 
		preBuild: 
			commands: 
				- > 
				  if [[ $AWS_BRANCH == "main" ]]; then 
					  echo $AWS_BRANCH;
					  mv .env.main .env 
				  elif [[ $AWS_BRANCH == release* ]]; then 
					  echo $AWS_BRANCH;
					  mv .env.release .env 
				  elif [[ $AWS_BRANCH == "develop" ]]; then 
					  echo $AWS_BRANCH;
					  mv .env.develop .env 
				  fi 
				- yarn add sharp --ignore-engines 
				- yarn install 
		build: 
			commands: 
				- yarn run build
	artifacts: 
		baseDirectory: build 
		files: 
			- '**/*'
	cache: 
		paths: 
			- node_modules/**/* 
			- build/cache/**/*
```
