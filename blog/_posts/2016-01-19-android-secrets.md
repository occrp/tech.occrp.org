---
layout: post
title: "Keeping Your Android Secrets Out of Git"
author: "Christopher Guess"
# categories: [ "Android", "Security", "Git" ]
---

Developers have a habit (one I've been guilty of) of committing API keys and other secrets to our repositories. It’s easy to do it if you’re tired, if you’re hurried, if you’re “moving fast and breaking things”.

This, unfortunately, has been too difficult to prevent for too long. In the interest of security there’s luckily been a big push to stop this practice lately; Rails has had the [Figaro](https://github.com/laserlemon/figaro) gem, but recently in version 4.1 they’ve built in a [“secrets.yml”](http://daniel.fone.net.nz/blog/2013/05/20/a-better-way-to-manage-the-rails-secret-token/) file. Heroku has a config [ENV](https://devcenter.heroku.com/articles/config-vars) screen to /attempt/ to coerce developers into keeping secret keys out of production. Apple’s iOS and its [keychain](http://www.raywenderlich.com/92667/securing-ios-data-keychain-touch-id-1password) helps with this on iPhones and iPad.

As far as I’ve been able to tell, Android has been terrible at this.

I’ve scoured documentation, searched for hours across StackOverflow and questioned friends who are much better at Android that I am. After taking bits and pieces, I think I’ve figure a good way to do this. I’m probably not the first, but there doesn’t seem to be a comprehensive write up of this technique anywhere, so I'm hoping these steps help fellow Android devs up their security a bit.

Note: this does not secure credentials in the wild. It will not stop someone from decompiling your ADK and pulling the string. Everything ends up in the compiled app. What it does do is keep someone from going through your Github account and copy/pasting your secrets out of it.

_Note: For these steps I’m assuming you’re using Android Studio._

1. Recognize what needs to be kept secret.

	- Anything that’s unique to your deployment of the software.
	- If you use a key for Google Analytics, or if you keep have an encryption key that needs to be hardcoded, these should never be committed to a repository.
	- Don’t put these into the source code, ever, even for brief testing purposes.

2. Create a Gradle file just for your keys

	- In Android Studio’s Project Navigator expand your “Gradle Scripts” drop down.
	- Right click anywhere below the “Gradle Scripts” icon and hover over “New” and then click “File”.
	- Name this file “safe\_variable.gradle” (or whatever you want, just make note of it if it’s different).

3. Add this file to your .gitignore.
	- We don’t want to accidentally add it to the repository so add the following line the bottom of your .gitignore file in the project:

		`/app/safe_variables.gradle`
4. Commit your .gitignore file

	`git commit .gitignore -m “Added secrets file to git ignore”`
5. Add your keys to the new secrets file.
	- This example uses two modes, debug and production. The names of the keys are arbitrary and you can put in whatever you need to keep secret.

	```
	buildTypes {
		debug{
			resValue "string", "server\_url", "https://dev.example.com"
			resValue "string", "hockey\_key", "\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*"
		}
		release {
			resValue "string", "server\_url", "https://production.example.com"
			resValue "string", "hockey\_key", "\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*"
		}
	}
	//This line is only necessary if your app is using localization files for the strings.
	//There doesn't seem to be a way to add the strings to multiple langauges from Gradle.
	lintOptions {
		disable 'MissingTranslation'
	}
	```
6. Include this file into your Gradle file
	- On the “build.gradle (Module: app)” file, add the following line right after the “android” block
	
		```
		apply from: 'safe_variables.gradle', to: android
		```
		
7. Commit your project again
8. Build your project
	- “Build” menu -\> “Make Project”
	- This will automatically add the files to compiled variable so you can reference it in your code.
9. Reference your API keys and other secrets where you need them.
	- In an Activity you can reference it like so:
	
	```
	getResources().getString(R.string.server_url)
	```
	
- In a fragment the following syntax can be used:

	```
	getActivity().getApplicationContext().getString(R.string.server_key)
	```

That should do it. The only thing to remember is that if you’re switching machines or adding a new teammate they’ll have to recreate steps 2 and 5 on their machine as well.

If there’s an easier way to do this or perhaps a way to do it without having to turn off the translations error please feel free to get in contact at [cguess@gmail.com](mailto:cguess@gmail.com) or on Twitter at [@cguess](https://www.twitter.com/cguess).