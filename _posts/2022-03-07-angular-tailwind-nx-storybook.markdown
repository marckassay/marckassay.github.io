---
layout: post
title:  "Set up Tailwind CSS and Storybook with Angular in an Nx workspace"
date:   2022-03-07 17:07:01 -0400
categories: workspace node
---

This post is a brief continuation of '[Set up Tailwind CSS with Angular in an Nx workspace](https://medium.com/nrwl/set-up-tailwind-css-with-angular-in-an-nx-workspace-6f039a0f4479)' by [Leosvel Pérez Espinosa](https://medium.com/@leosvel). The following repository forked from the blog post just mentioned contains all that is mentioned below:

- [https://github.com/marckassay/angular-tailwind-storybook-nx](https://github.com/marckassay/angular-tailwind-storybook-nx)

The objective of this blog post is to demonstrate integrating Storybook in 'app1' and 'lib1' of this workspace. And also, to demonstrate how to set Angular's template binding and `ng-content` in a story. This image shows the results of installing and configuring Storybook for 'app1':

<div style="display: flex;justify-content: center; padding: 2em;">
  <a href="/assets/2022-03-07/app1.stories-storybook.png"><img title="updated app1 serving storybook" style="box-shadow: 3px 3px 5px rgba(0, 0, 0, .7);" src="/assets/2022-03-07/app1.stories-storybook.png" /></a>
</div>

## Configure 'app1' for Storybook

Regardless of the package manager you're using, generate Storybook files and modifications using `npx`:

```shell
npx nx g @nrwl/angular:storybook-configuration app1
```

Opting out of both options in the command prompt to include Cypress tests, should yield something similar to commit [3b531ded](https://github.com/marckassay/angular-tailwind-storybook-nx/commit/3b531dec45db341876a291f076077cac7e6f5585). With that SHA, execute `git show 3b531de --name-status` to see what changed:

<script src="https://gist.github.com/marckassay/cc91669e97a0b74221c294abaab6c068.js"></script>

Now let's modify some files to make use of Storybook. Starting with `project.json` file, add app1's Tailwind `styles.css` file to Nx project file:

<script src="https://gist.github.com/marckassay/b5e3233570680270a1a150eaee73f0bd.js"></script>

Removing the auto-generated 'NxWelcomeComponent' file and story, since it's not needed, modify `app.component.html` so that it supports 4 columns of cards instead of 3. The fourth column card provides information about Storybook. This is shown in the image above.

<script src="https://gist.github.com/marckassay/6be30a23bc0f0148650da5733dfc5af6.js"></script>

And also modify app1's component file:

<script src="https://gist.github.com/marckassay/57cca477d150c38331485888b594a0cf.js"></script>

From the perspective of 'app1', the typography that is rendered for `angular-tailwind-nx-header` component, can be considered a `title`. And from the perspective of 'lib1', this wording can be seen as content. Change 'lib1' to support this change:

<script src="https://gist.github.com/marckassay/7b575d18f97a447413c5a56164850c5a.js"></script>

And also, modify the generated story for 'app1':

<script src="https://gist.github.com/marckassay/5f88fe28eb2251cd6a9c7b1aaf4667bd.js"></script>

And finally, make appropriate changes in `app.module.ts` file; remove 'NxWelcomeComponent'.

These changes, make app1's story more practical, although modest. The changes should now be able to yield what is seen in the image above by executing:

```
npx nx storybook app1
```

To view this section's commit: [46aff68](https://github.com/marckassay/angular-tailwind-storybook-nx/commit/46aff6889525fbd0614c89344174813e0157a76f).


## Configure 'lib1' for Storybook

Similarly with generating Storybook for 'app1', do the following for 'lib1':

```shell
npx nx g @nrwl/angular:storybook-configuration lib1
```

If there is part of this post that may be debatable, it will likely be in the changes of lib1's `project.json` file. Here is what changed:

<script src="https://gist.github.com/marckassay/501d427426af1b820b4d06e6104f1435.js"></script>

For `storybook` command of this library, notice lines 8 and 13. This library is using app1's configuration for Tailwind and Storybook. At this moment, I'm content with this configuration, although you may desire an alternative. The same change to `storybook` command is applied to the `build-storybook` command of that same file.

Modify `header.component.html` by moving current css into it component css file:

<script src="https://gist.github.com/marckassay/5884dfa6014e74088d7bb5a43d9288e2.js"></script>

Also, you may have noticed a new property `strong` for this component. This is another modest change to make this component's story have a little more meaning.

For the sake of creating 2 Gists for this component's css and ts file changes, below are both:

<script src="https://gist.github.com/marckassay/8e5081a82e6c85b11c8bfa90db208326.js"></script>

The `strong` property is added to a new interface, `Header`, that will be used to enforce data type in its story.

Now for the its story:

<script src="https://gist.github.com/marckassay/dd2e01fc61d158a9dfd3b165cc8ac84c.js"></script>

In the previous section, _'Configure 'app1' for Storybook'_, we changed the `title` property of this component to be a value for its `ng-content`. That was intentional to demonstrate how to set an Angular component's _content_ value in a story. And for simplicity, perhaps not an ideal location, I added a `Content` interface that is [_intersected_](https://www.typescriptlang.org/docs/handbook/2/objects.html#intersection-types) with `Header` to form a new type, `HeaderContent`. We use this type to be passed into the `Template`, which adds `HeaderComponent` to the DOM, and sets its input value `strong`.

To serve 'lib1':

```
npx nx storybook lib1
```

This image shows the results of this section:

<div style="display: flex;justify-content: center; padding: 2em;">
  <a href="/assets/2022-03-07/lib1.stories-storybook.png"><img title="updated lib1 serving storybook" style="box-shadow: 3px 3px 5px rgba(0, 0, 0, .7);" src="/assets/2022-03-07/lib1.stories-storybook.png" /></a>
</div>

Notice the absence of cards which we saw in the image above as this story is just for 'lib1', and `CardComponent` is in 'lib2'. This fashion of development, isolating components to their own environment for testing, is the essence of [Component Driven](https://www.componentdriven.org/) development.

To view this section's commit: [0075f1ad](https://github.com/marckassay/angular-tailwind-storybook-nx/commit/0075f1ad9ee0dda27be27b857db4e27f444b0678).

## Conclusion

Hopefully, this post demystifies some uncertainties you might have had with integrating Storybook into Leosvel Espinosa's workspace, or any other Nx Angular workspace, from his blog [post](https://medium.com/nrwl/set-up-tailwind-css-with-angular-in-an-nx-workspace-6f039a0f4479). I believe this stack of techs: Angular, Tailwind, Nx, and Storybook, together can be advantageous to the development and longevity of an application(s).
