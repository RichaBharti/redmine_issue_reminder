﻿Redmine Issue Reminder
==============

[![Join the chat at https://gitter.im/Hopebaytech/redmine_issue_reminder](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/Hopebaytech/redmine_issue_reminder?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

Compatible with redmine 2.6.1.stable

Plugin provides an easy to use interface to set up automatic email reminder to every project. 
Every reminder uses a custom query with all their filter options to select issues 
and performs periodical email transmission on a role basis.

Following intervals are possible:
 - Daily (Selecting interval from every 1st day until every 6th day)
 - Weekly (Selecting weekday)
 - Monthly (Selecting day of the month)

## Screenshots

![](http://farm7.static.flickr.com/6109/6294745006_49986ec541_b.jpg)
![](https://lh3.googleusercontent.com/-A-s0giWSvvk/VNiFf7PnSfI/AAAAAAAACHI/6DA5JDDmF2U/s2048/2015-02-09%25252018_01_30-2015-02-09%25252017_52_56-Redmine%252520Issue%252520Reminder%252520-%252520jethro.yu%252540happygorgi.com%252520-%252520%2525E5%252592%25258C%2525E6%2525B2%25259B%2525E7%2525A7%252591%2525E6%25258A%252580%2525E8%252582%2525A1%2525E4%2525BB%2525BD%2525E6%25259C%252589%2525E9%252599%252590.png)

## Installation - Linux

Download the sources and put them to your vendor/plugins folder.

```console
$ cd {REDMINE_ROOT}/plugins
$ git clone https://github.com/Hopebaytech/redmine_issue_reminder.git
```

Install required gem for plugin

```console
$ bundle install
```

Install plugin and update DB

```console
$ rake redmine:plugins:migrate
```

(See also http://www.redmine.org/projects/redmine/wiki/Plugins )    

For the periodical transmission a daily cron job has to be created:

If you use system ruby:

```console
$ sudo crontab -e
0 6 * * * cd {REDMINE_ROOT} && rake reminder:exec RAILS_ENV="production" > /dev/null 2>&1
```

If you use RVM:
```console 
$ sudo crontab -e
0 6 * * * {REDMINE_ROOT}/script/mail_reminder.sh > /dev/null 2>&1
```

```console 
$ vim {REDMINE_ROOT}/script/mail_reminder.sh
#!/bin/bash
source {USER_HOME}/.rvm/scripts/rvm
export PATH="$PATH:{USER_HOME}/.rvm/bin"
cd {REDMINE_ROOT}
rake reminder:exec RAILS_ENV=production
```

Restart Redmine

Run Redmine and have a fun!

The reminder functionality can be activated in each project as module and can be configured through the project menu entry "Reminder Settings".
A special right needs to be configured in order to allow project member to edit reminder.

## Installation - Windows

Enviroment : Winxp + Redmine 1.2.X + Mysql 5.X
 
 1. Write a bat file such as these

```bat
echo on
cd {REDMINE_ROOT}
rake reminder:exec RAILS_ENV="production"
```

 2. config a schedule just follow this
 http://www.iopus.com/guides/winscheduler.htm
 
 3. then start the redmine server.
 
## Testing redminder mail

To send test mail without inverval check:

```console
rake reminder:exec[test]
```

 `rake reminder:exec[test]` is supposed to have exactly the same behavior as `rake reminder:exec` except two things :
 
* it does always send emails (no matter when the last execution was)
* it does not update the last execution date

The behavior of `rake reminder:exec` is to send email only if it is time to send a new email, regarding the interval parameters and the `rake reminder:exec[test]` is supposed to send email each times it is executed with a non empty body.

## Troubleshouting

### How can i customize the queries?

Take a look at the official documentation about custom queries: 
http://www.redmine.org/projects/redmine/wiki/RedmineIssueList#Custom-queries

### I don't see the Reminder Settings

Add permission to Your user.

### The issue reminder doesn't send mails

We use redmine internal mail send functions, therefore the outgoing email settings 
has to be set in config/emai.yml or config/configuration.yml

### I can not use the windows scheduler (WinXP related)

You need to have a user password set for your windows user in order to use the windows scheduler.

(See also here: http://technet.microsoft.com/en-us/library/cc785125(WS.10).aspx )

## Translations

- de by Michael Kling
- en by Boško Ivanišević
- sr-YU by Boško Ivanišević
- sr by Boško Ivanišević

Thanks for the contribution. 

## To-Do List

- Fix: changing interval type updates interval value @ "New reminder"
- Fix: cancel button @ "Edit"
- Fix: load value per reminder @ "Edit"
- Feature: `rake reminder:exec[reset_scheduler]`

## Changelog

### 2014/6/3

 - Inline CSS from site setting, What You See Is What You Get. Require `$ bundle install`

### 2014/5/23

 - Add a way to test by rake command
 - User can use all queries viviable at redmine page to set reminder.
 - Fix async_smtp can't
 - Fix compatibility with Redmine 2.5.1

### 0.0.1

 - initial release
 - matches the basic requirements
