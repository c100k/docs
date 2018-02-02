---
---

{! ingredients/email/email-configure-esp.md !}

Contact your Appboy Account Manager and request the Email Click Tracking Domain and the SendGrid Data domain associated with your SendGrid account.

Once you’ve retrieved this information, enter both the click tracking domain and the SendGrid domain in item 1 of this step: 

![image](/img/pages/email/sendgrid/configure-sendgrid-1.png)

On **Done** click, an AASA file - required for Universal Links - specific to that domain will be generated.

{! ingredients/email/email-technical-setup.md !}

{! ingredients/email/email-cname.md !}

{! ingredients/email/email-associated-domains.md !}

{! ingredients/email/email-validate-test.md !}

{! ingredients/email/email-usage.md !}

{! ingredients/email/email-link-options-menu.md !}

**Appboy uses the shortcode `e_ab` for links in emails** - please use this in place of `e_xx` in the guide below.

{! ingredients/email/email-link-options.md !}

### Flag your deep links

To use Branch links within your appboy campaigns, you’ll need to add an HTML tag to the Branch URLs within your appboy email templates.

1. Create your email template same as you normally would.
1. Add Branch links to your email for all links you would like to deep link users to specific app content (this works for existing app users as well as those without your app). Be sure to add `"$3p":"e_ab"` to the deep link data of any links you use in email to ensure Universal Link and click tracking works as expected.
1. Once you’re done composing your email template, click on the “Source” button in the email creator toolbar.

    ![image](/img/pages/email/appboy/appboy-source.png)

1. Add universal="true" to the HTML for any link which you want to convert intoa Branch deep link, for example:

    ```html
    <a href="links.example.com" universal="true">Link to your app!</a>
    ```

    ![image](/img/pages/email/appboy/appboy-universal.png)

All done! So long as you’ve taken the above steps, the links in your Appboy email campaigns will now dynamically deep link users to app content, even if they have uninstalled your app, giving you the best chance of engaging or re-acquiring them.

{! ingredients/email/email-support.md !}
