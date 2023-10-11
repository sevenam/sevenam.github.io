# cognitiveoverload

```bash
# documentation: https://stack.jimmycai.com/guide/getting-started
# github actions deployment: https://gohugo.io/hosting-and-deployment/hosting-on-github/

# create new site
hugo new site cognitiveoverload

# add theme as submodule
git submodule add https://github.com/CaiJimmy/hugo-theme-stack/ themes/hugo-theme-stack

# copy from 
# themes\hugo-theme-stack\exampleSite
# to the "content" folder

# add theme to config in the "hugo.yaml" file:
theme = 'hugo-theme-stack'

# serve with hugo localy on port 1313
hugo serve -p 1313

# configure the site in the hugo.yaml file
# see config example at: themes/hugo-theme-stack/exampleSite/config.yaml

```

## Adding Disqus

Create a disqus.com account, create a site and add the following in the config.yaml:

```yaml
disqusShortname: name-of-your-disqus-site

params:
    comments:
        enabled: true
        provider: disqus



```