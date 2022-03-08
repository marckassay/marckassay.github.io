---
layout: post
title:  "Set up Tailwind CSS and Storybook with Angular in an Nx workspace"
date:   2020-03-07 17:07:01 -0400
categories: workspace node
---

This post is a brief continuation of '[Set up Tailwind CSS with Angular in an Nx workspace](https://medium.com/nrwl/set-up-tailwind-css-with-angular-in-an-nx-workspace-6f039a0f4479)' by [Leosvel Pérez Espinosa](https://medium.com/@leosvel). The following repository forked from the blog post just mentioned, contains all what is mentioned below:

- https://github.com/marckassay/angular-tailwind-storybook-nx

Objective for this blog post is to demonstrate integrating Storybook in 'app1' and 'lib1' of this workspace. This image shows the results of installing and configuring Storybook for 'app1':

![updated app1 serving storybook](/assets/2022-03-07/app1.stories-storybook.png)

## Prep work needed

During the time of this post, I choose to update Nx using its [`migrate`](https://nx.dev/cli/migrate) command:

```shell
yarn nx migrate @nrwl/workspace@latest
```

As a result, this command generated a `migrations.json` file in the workspace root directory.

And for Window users, convert `rm` command to node's equivalent:

<script src="https://gist.github.com/marckassay/2fa5b3e331be92988cf6a562302e482c.js"></script>

## Configure 'app1' for Storybook

Regardless of package manager you're using, execute the following using `npx`:

```shell
npx nx g @nrwl/angular:storybook-configuration app1
```

This should yield something similar to commit [`3b531de`](https://github.com/marckassay/angular-tailwind-storybook-nx/commit/3b531dec45db341876a291f076077cac7e6f5585). With that SHA, execute `git show 3b531de --name-status` will output the status of it:

```
commit 3b531dec45db341876a291f076077cac7e6f5585
Author: Marc Kassay <marckassay@gmail.com>
Date:   Mon Mar 7 12:51:57 2022 -0500

    generate storybook for app1

    - Executed: npx nx generate @nrwl/angular:storybook-configuration app1
    - Opt-in only for 'stories files for components declared in project'

A       .storybook/main.js
A       .storybook/tsconfig.json
A       apps/app1/.storybook/main.js
A       apps/app1/.storybook/preview.js
A       apps/app1/.storybook/tsconfig.json
M       apps/app1/project.json
A       apps/app1/src/app/app.component.stories.ts
A       apps/app1/src/app/nx-welcome.component.stories.ts
M       apps/app1/tsconfig.app.json
M       apps/app1/tsconfig.json
M       nx.json
M       package.json
M       yarn.lock
```

Now let's modify some files to make use of Storybook. Starting with `project.json` file, add app1's Tailwind preset file to Nx project file:

<script src="https://gist.github.com/marckassay/b5e3233570680270a1a150eaee73f0bd.js"></script>

Removing the auto-generated 'NxWelcomeComponent' file and story, since it's not needed, modify `app.component.html` so that it supports 4 columns of cards instead of 3. The fourth column card provides information about Storybook. This is shown in the image above.

<script src="https://gist.github.com/marckassay/6be30a23bc0f0148650da5733dfc5af6.js"></script>

And also modify app1's component file:

<script src="https://gist.github.com/marckassay/57cca477d150c38331485888b594a0cf.js"></script>

From the perspective of 'app1', the wording that is rendered for `angular-tailwind-nx-header`, can be considered a `title`. And from the perspective of 'lib1', this wording can be seen as content. Change 'lib1' to support this change:

<script src="https://gist.github.com/marckassay/7b575d18f97a447413c5a56164850c5a.js"></script>

And also, modify the generated story for 'app1':

<script src="https://gist.github.com/marckassay/5f88fe28eb2251cd6a9c7b1aaf4667bd.js"></script>

And finally, make appropriate changes in `app.module.ts` file; remove 'NxWelcomeComponent'.

With these changes, it makes app1's story more practical, although modest. The changes should now be able to yield what is seen in the image above by executing:

```
npx nx storybook app1
```

To view the complete commit here: [46aff68](https://github.com/marckassay/angular-tailwind-storybook-nx/commit/46aff6889525fbd0614c89344174813e0157a76f).


## Configure 'lib1' for Storybook



