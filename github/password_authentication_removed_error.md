
# Problem statement:

I pulled one of the [repos](https://github.com/litoco/SmallProjects) from my github to add a new learning project. I added the project code to the this repo and then tried push it only to get an error message `Support for password authentication was removed on August 13, 2021` on my terminal. I immediately remembered that I had faced the same error before but this time I thought to document it for my own reference.

# Solution:

The error had provided a [link](https://docs.github.com/en/get-started/getting-started-with-git/about-remote-repositories#cloning-with-https-urls) for reference. I skimmed through the documentation and found out two solutions:\
either,\
use [Git Credential manager](https://github.com/git-ecosystem/git-credential-manager/blob/main/README.md) that would facilitate authentication step. Basically it ask for the credentials for the first time login and uses them there after\
or,\
you can use [personal access tokens](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens) as passwords.


I followed the second option. In this we needed to follow some steps and our personal access token would be generated for use. The steps to followed are:

1. [creating a fine grained personal access token (in beta and subject to change)](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#creating-a-fine-grained-personal-access-token)
2. [creating a personal access token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#creating-a-personal-access-token-classic)

Both of these approaches have different pros and cons. You can read about them from [fine-grained personal access tokens](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#fine-grained-personal-access-tokens) and [Personal access tokens](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#personal-access-tokens-classic).
