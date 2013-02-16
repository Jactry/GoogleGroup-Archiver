Google Group Archiver
---------------------

This is a small tool to create a HTML archiver for an existing Google Group, which can import existing mails and update new mails regularly.

Dependencies:
  * Python 2
  * GNU Mailman
  * Gmail

HOWTO
-----

1. Prepare Group Subscriber
  * Create a Gmail account (do not use a precious password)
  * Subscribe to your Google Group
  * Create a filter that labels to:(listname@googlegroups) as "listname"

2. Setup Mailman
  * Install GNU Mailman
  * Apply the modifications if you want (see below).
  * Create a new list (admin mail and password do not matter):
    `/usr/lib/mailman/bin/newlist`

3. Import History
  * python migrate.py
  * choose "d" to download from YOUR Gmail account
  * choose "m" to convert box format to mbox format
  * (You can use any other E-mail client to download mbox)
  * `/usr/lib/mailman/bin/arch listname mbox-file`

4. Setup Crontab
  * Modify cron.sh script, set variables LISTS, EMAIL and PASSWORD
  * Add crontab to a privileged user (e.g. root) which runs cron.sh every 15 minutes.

5. Setup Web Server

Nginx for example:
<code>
        location /pipermail {
                alias /var/lib/mailman/archives/public;
                autoindex on;
        }
</code>


Modifications made to Mailman:
  * GBK handling:
    HyperArch.py => /usr/lib/mailman/Mailman/Archiver/HyperArch.py
  * Remove unused HTML elements:
    template => /var/lib/mailman/templates/en/

If you modified the templates, you have to rebuild HTMLs:
  * /usr/lib/mailman/bin/arch --wipe listname history-mbox-file
  * python migrate.py to convert listname-full.box (This contains all mails received by cron.sh) to mbox format
  * /usr/lib/mailman/bin/arch listname listname-full.mbox