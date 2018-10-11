# EXAMPLE_1 (Autodiscover via DNS)

Setup Amazon WorkMail:
  - for organization `xbs-united` (so e-mail available via browser via [https://xbs-united.awsapps.com/mail](https://xbs-united.awsapps.com/mail))
  - for (already existing) domain `xburser.com`
  - for user `xburser`
  - AutoDiscover via DNS record (so Phase 3 from [here](https://docs.aws.amazon.com/workmail/latest/adminguide/autodiscover.html))

(so working e-mail `xburser@xburser.com` has to be as a result)




## SETUP YOUR ORGANIZATION

Choose necessary **Region**.

Go to the **WorkMail** service.

Click **Get started** button.

Click **Quick setup** button.

```
Organization name: xbs-united
```

Click **Create** button.





## ADD YOUR DOMAIN

Click on the link of the just created organization: `xbs-united`.

Navigate to **Domains**.

Click **Add domain** button.

```
Domain name: xburser.com      <== enter here some your domain OR buy a new one
```

Click **Add domain** button.


After Amazon verifies the domain ownership, it can be used in email addresses for users, groups and resources.





## VERIFY ADDED DOMAIN



### VERIFY DOMAIN OWNERSHIP

Before you can use your domain with Amazon WorkMail, Amazon needs to verify the domain ownership.

Following steps are fully described on the WorkMail verifying web-page during e-mail configuration.

**Add the following record to your DNS hosting provider**

Go to the **Route 53** service.

Click **Hosted zones** link.

Click your domain (`xburser.com` in this case).

Click **Create Record Set** button.
```
Name: _amazonses.xburser.com.
Type: TXT - Text
Alias: No radiobutton is selected.
Value: NycvWm2dfCV1k=       <== as described on appropriate WorkMail config step
Routing Policy: Simple
```

Click **Create** button.

> Note: If you delete the TXT record from your DNS hosting provider, this domain will not be able to send and receive emails any longer.

Optional: click **Check verification** link in some time to see **Verified** status eventually.





### FINALIZE DOMAIN SETUP

To switch your domain completely to Amazon WorkMail, add the following DNS records to your DNS hosting provider.

If your domain already has email addresses, be careful when you change MX records. 

To avoid email service disruption, make sure that all your user accounts, distribution lists and resources are added.


Following steps are fully described on the WorkMail verifying web-page during e-mail configuration.


> Note: Remove any existing MX records before adding the new MX record.

Click **Create Record Set** button.
```
Name: xburser.com.
Type: MX
Alias: No radiobutton is selected.
Value: 10 inbound-smtp.us-east-1.amazonaws.com.      <== as described on appropriate WorkMail config step
Routing Policy: Simple
```

Click **Create** button.



Click **Create Record Set** button.
```
Name: autodiscover.xburser.com.
Type: CNAME
Alias: No radiobutton is selected.
Value: autodiscover.mail.us-east-1.awsapps.com.      <== as described on appropriate WorkMail config step
Routing Policy: Simple
```

Click **Create** button.



Click **Create Record Set** button.
```
Name: fdwr2y2br._domainkey.xburser.com.
Type: CNAME
Alias: No radiobutton is selected.
Value: fdwvr.dkim.amazonses.com.      <== as described on appropriate WorkMail config step
Routing Policy: Simple
```

Click **Create** button.


Click **Create Record Set** button.
```
Name: e3twfcqhxkoa._domainkey.xburser.com.
Type: CNAME
Alias: No radiobutton is selected.
Value: e34koa.dkim.amazonses.com.      <== as described on appropriate WorkMail config step
Routing Policy: Simple
```

Click **Create** button.



Click **Create Record Set** button.
```
Name: otx3i2pnadd3._domainkey.xburser.com.
Type: CNAME
Alias: No radiobutton is selected.
Value: otxpd3.dkim.amazonses.com.      <== as described on appropriate WorkMail config step
Routing Policy: Simple
```

Click **Create** button.

Click **Close** button.

> Note: It can take up to 72 hours before all DNS records are propagated.


### EVENTUALLY

Your domain `xburser.com` has to have **Verified** status.

> It took me 5 min to get that status.





## ADD USER ACCOUNT

After successful verification we are now ready to create email accounts.

Click on **Users** on the left menu and click on **Add user**.
```
User name: xburser
First name: Sergii
Last name: Burtovyi
Display name: Sergii Burtovyi
```

Click **Next Step**.

Provide the primary email address and password of the new user.
```
Email address: xburser@xburser.com    <== "xbs-united.awsapps.com" may be used too
Password:
Repeat password:
```

Click **Add user**.



## VERIFY EMAIL

Go to [https://xbs-united.awsapps.com/mail](https://xbs-united.awsapps.com/mail).

Login as just created `xburser@xburser.com`.

Try to send/receive mails.




















