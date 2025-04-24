Instructions for creating new posts.

1. There's two things to know. There's a "homepage" for posts which usually consists of lists of posts
and there are the posts themselves.

2. Homepages (at the time of writing this) are:
  - blog.md
  - index.md
  - writeups.md
  - books.md
  - projects.md

3. Each homepage should have a corresponding folder for its posts entited *name of the homepage*. EX: There's a writeups homepage, so there should be a writeups folder.

4. The posts folder can have subfolders depending on whether the current category can have subcateogories. It's not necessary to have subcategories. For example: the writeups category has two subcategories: "algorithms" and "chess". These are separate subfolders in the writeups folder.

5. In order to actually create a post, you must create a "_posts" folder in each category/subcategory folder. Inside that "_posts" folder, you can create posts.

6. Post structure must be in format YYYY-MM-DD-title-of-your-post.md

7. To add the list of posts to your homepage, include the post_list layout and specify the category and optional subcategories. See blogs.md for an example with just category posts, and see writeups.md for an example with subcategories.