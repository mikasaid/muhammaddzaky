Sitemap
The site has a simple structure ... basically two-level (homepage and then content pages). There are a couple of additional pages, for related but subordinate information, accessible from the footer navbar.

Appropriate comments are found in each of the controllers.

Project Folders
/application	the obvious
/data	zipped files for downloading, avatars
/public	the served face of the website
/public/assets CSS, javascript & media /public/user_guide Symbolic link to the current CI user guide (v3) /public/userguide2 Placeholder for the CI2 user guide /public/userguide3 Placeholder for the CI3 user guide /tests Provision for unit testing (not used) /writable Writable folder for temporary files

You may have noticed that there is no system folder. That is on purpose ... the website assumes that the CodeIgniter4 framework has been installed locally at the same level as the website project.
