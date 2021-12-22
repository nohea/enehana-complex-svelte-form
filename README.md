# enehana-complex-svelte-form

source code accompanying the tech blog post at:
http://enehana.nohea.com

# overview 

Building new web apps in 2021 using a Svelte front-end is fun, with more reactivity and less code. Almost any web app will have some kind of form, and it helps to have a basic form builder and validation framework.

In this post, i’ll be exploring the svelte-forms-lib library to create a form, bound to a hierarchical object, and also wired up to a validation object. The form will support dynamically adding/removing items from an array property. It will also support radio buttons, which must be handled differently, since they are multiple <input> elements tied to the same variable. 

https://github.com/tjinauyeung/svelte-forms-lib
