---
layout: doc
title: Configure GitHub
permalink: /docs/configure-github/
---

To get your version control up and running as well as the GitHub Actions that automatically deploy your application, follow the below steps.

1. Create a GitHub repository either under your personal GitHub account or a GitHub organization by following the [official GitHub documentation](https://docs.github.com/en/get-started/quickstart/create-a-repo).
2. Navigate to the project folder containing your Volca application and update the file `volca.config.ts` with the following values

```ts
...
github: {
    organization: 'my-github-org', // Your GitHub organization or username
    repository: 'my-github-repo', // Your GitHub repository name
},
...
```

3. Run the below command in your preferred terminal to push the code to your newly created repository.

Replace `GITHUB_ORGANIZATION` with either your GitHub username if you created the repository under your user or your organization if you created it under an organization.

Replace `GITHUB_REPOSITORY_NAME` with your repository name.

```
git remote add origin git@github.com:<GITHUB_ORGANIZATION>/<GITHUB_REPOSITORY_NAME>.git
git branch -M main
git push -u origin main
```

3. Done! Note that some GitHub Actions may fail the first time you push your code. These will start working once you have gone through the [deploy infrastructure](/docs/deploy-infrastructure/) guide
