OS 499 Scripts
===============

Authentication
--------------

### .netrc (for svn)

* Create ~/.netrc with your WatIAM credentials to log into the SVN server

    machine ecesvn.uwaterloo.ca login <USERNAME> password <PASSWORD>

* Set the correct permissions on ~/.netrc (Note: This won't save you from untrusted sysadmins on that box.)

    $ chmod 0600 ~/.netrc

### SSH keys (for git)

* Create a password-less SSH key for use for mirroring

    $ ssh-keygen -f ~/.ssh/id_rsa_os499_github

* Add the contents of the _public_ key to a GitHub account with R/W access to the OS 499 repositories (presumably yours, although a separate bot account would theoretically do). (Don't forget the .pub. :)

    $ cat ~/.ssh/id_rsa_os499_github.pub

    ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDFJoUl9JvSemQWZVMNlw8bT8WqUel+Lq90MIFg6uaFi8yn0QX8tT8YmDrh8g4pmIkX1vFuJCh2H397//Ff19QoLFze4kz5kRpML8qniW2SF/qmGyKn34PaUnhAuoJPFXiNq3JcIg8Qx8wdwNMpSjUvMGemFcwZYgGUDdwhsu1pV7tz47fDNcBW9WSl+bO0rG+xNFWMALiaVb71s8mqjCjiPoQVJblvyE0iuBdhk8kY7IaiFUx/b73BWS2elfpmLCvtMoo0DKVF4elo5YcirllD6IMqO44YYkjQRdYngaz5gmbXGLOJKD5P+WouTKcH08Oww2VhJwrsfTbb2AKGprTJ michael@paprika

* Create or amend ~/.ssh/config to enable automated login with the correct key

    Host os499mirror
    User git
    Hostname github.com
    IdentityFile /home/michael/.ssh/id_rsa_os499_github

* Set the correct permissions on ~/.ssh/config

    $ chmod 0600 ~/.ssh/config

Creating a Repository
---------------------

### Dependencies

On recent verisons of Ubuntu (tested on 11.10 as of the time of writing),

    $ sudo aptitude install subversion git git-svn

Note that git used to be called git-core on older Debian-based distributions. For other distributions, YMMV.

### Create "repository" in SVN

* NB. This just creates a folder under the single master os499 repository. (FIXME: Perhaps there is a way to make a proper "child" repository; if so, need to see if it's appropriate for our use.)

* Check out os499 from ecesvn

    $ mkdir ~/svn/
    $ cd ~/svn/
    $ svn co https://ecesvn.uwaterloo.ca/courses/os499

NOTE: The SVN repository was initially located at [https://ecesvn.uwaterloo.ca/people/os499][] .

* Make the new "repository" (called scripts here); add a README file. (Markdown for illustration because that's what this file uses.)

    $ cd os499
    $ mkdir scripts
    $ mkdir scripts/trunk
    $ touch scripts/trunk/README.markdown
    // ... vim scripts/trunk/README.markdown (optional)

* Commit to SVN (this pushes to the server at the same time)

    $ svn add scripts
    $ svn commit

### Set up mirror on GitHub for non-public repos

Note that if the repo is public, you can just use GitHub's mirroring feature.

* Create the project on GitHub

* Initial set-up (git)

    $ mkdir ~/git/
    $ mkdir ~/git/scripts/
    $ cd ~/git/scripts/
    $ git init

* Create svn.authors

    m9chang = Michael Chang <m9chang@uwaterloo.ca>
    <WatIAM username> = <full name> <uwaterloo email wrapped in pointy brackets> 

* Initial sync

    $ git svn init -T https://ecesvn.uwaterloo.ca/courses/os499/scripts/trunk/
    Using higher level of URL: https://ecesvn.uwaterloo.ca/courses/os499/scripts/trunk => https://ecesvn.uwaterloo.ca/courses/os499
    $ git svn fetch --authors-file=svn.authors
    $ git gc

* Initial push (host here should match the Host line in .ssh/config that you created earlier)

    $ git remote add origin git@os499mirror:os499/scripts
    $ git push origin master

Keeping a Repo Mirror Up-to-Date
--------------------------------

### SVN -> Git

    $ git svn rebase --authors-file=svn.authors
    $ git push origin master

### Git -> SVN (FIXME)

    $ git pull origin master
    $ git svn dcommit

### Changes in both locations (FIXME)

    $ git svn rebase --authors-file=svn.authors
    $ git pull origin master
    // ... resolve merge issues in Git
    $ git push origin master
    $ git svn dcommit

### SVN URL changes (FIXME)

* [GitSvnSwitch][]
[GitSvnSwitch]: <https://git.wiki.kernel.org/index.php/GitSvnSwitch>

### Crontab

TODO

### Jekyll

TODO

Further Reading
---------------
* [OS 499][]
* [OS 499 Github][]
* [HOWTO iCoreTech][]
* [HOWTO petersteinberger.com][]
* [GitHub Pages][]
* [Jekyll Wiki][]

[OS 499]:                       <https://ece.uwaterloo.ca/~os499/>        "Official OS 499 Page"
[OS 499 Github]:                <http://os499.github.com>                 "OS 499 Page on GitHub"
[HOWTO iCoreTech]:              <http://www.icoretech.org/2009/08/how-to-mirror-a-svn-repository-on-github/> "How to mirror a SVN repository on GitHub (iCoreTech Research Labs)"
[HOWTO petersteinberger.com]:   <http://petersteinberger.com/2010/01/how-to-mirror-an-svn-repository-on-github/> "How to Mirror an SVN repository on GitHub (http://petersteinberger.com)"
[GitHub Pages]:                 <http://pages.github.com/>                "GitHub Pages"
[Jekyll Wiki]:                  <https://github.com/mojombo/jekyll/wiki>  "Jekyll Wiki"

