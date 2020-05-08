# Wiki

## Structure

## Deployment Workflow
`sudo pip install mkdocs`

#### Manual
###### Vanilla
1. `mkdocs gh-deploy`
2. git commit & push `master`

###### User/Org Pages
1. `cd /build`
2. `mkdocs gh-deploy --config-file ..\mkdocs.yml --remote-branch  master`
3. git commit & push `develop`

###### Custom Domain
[StackOverflow Answer](https://stackoverflow.com/questions/9082499/custom-domain-for-github-project-pages)

1. `mkdocs gh-deploy`
2. git commit & push `master`
3. Specify custom domain in GitHub Pages settings
4. Add `A` recorsd for github.io
    - 185.199.108.153
    - 185.199.109.153
    - 185.199.110.153
    - 185.199.111.153
5. Add `CNAME` record to point domain to GitHub Page `user.github.io/repo_name`
6. Enforce HTTPS

#### Build Server
###### Self-hosted