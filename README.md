# emailkit-submission-issue

Hello,

There are issues with your plugin code preventing it from being approved 
immediately. We have pended your submission in order to help you correct 
all issues so that it may be approved and published.

We ask you read this email in its entirety, address all listed issues, 
and reply to this email with your corrected code attached (or linked). 
You have three (3) months to make all corrections, before your plugin 
will be rejected. Even so, as long as you reply to this email, we will be 
able to continue with your review and eventually publish your code.

Remember in addition to code quality, security and functionality, we 
require all plugins adhere to our guidelines. If you have not yet, please 
read them:

https://developer.wordpress.org/plugins/wordpress-org/detailed-plugin-guidelines/

We know it can be long, but you must follow the directions at the end as 
not doing so will result in your review being delayed. It is required for 
you to read and reply to these emails, and failure to do so will result 
in significant delays with your plugin being accepted.

Finally, should you at any time wish to alter your permalink (aka the 
plugin slug), you must explicitly tell us what you want it to be. Just 
changing the display name is not sufficient, and we require to you 
clearly state your desired permalink. Remember, permalinks cannot be 
altered after approval.

Be aware that you will not be able to submit another plugin while this 
one is being reviewed.

## Nonces and User Permissions Needed for Security

Please add a nonce to your POST calls to prevent unauthorized access.

Keep in mind, check_admin_referer alone is not bulletproof security. Do 
not rely on nonces for authorization purposes. Use current_user_can() in 
order to prevent users without the right permissions from accessing 
things.

If you use wp_ajax to trigger submission checks, remember they also need 
a nonce check.

You also must avoid checking for post submission outside of functions. 
Doing so means the check runs on every single load of the plugin which 
means every single person who views any page on a site using your plugin 
will check for a submission. Doing that makes your code slow and unwieldy 
for users on any high-traffic site, causing instability and crashes.

The following links may assist you in development:

    https://developer.wordpress.org/plugins/security/nonces/
    https://developer.wordpress.org/plugins/javascript/ajax/#nonce
    https://developer.wordpress.org/plugins/settings/settings-api/


Example(s) from your plugin:

--------------------------------------------
LINE 230: ERROR Processing form data without nonce verification. 
(WordPress.Security.NonceVerification.Missing)

--------------------------------------------
   228:   
   229:  
········if·(isset($_POST['post_for_update'])·&&··$_POST['post_for_update']·!=·'')·{
>> 230:  ············$post_id·=$_POST['post_for_update'];
   231:  ········}
   232:

--------------------------------------------



## Your admin dashboard has an iframe

Having the admin dashboard include an iframe isn't permitted in the 
majority of cases for two main reasons - security and appearance. 
Instead, we recommend you change your code to use an API or just link 
back to your site so they can configure things there.

If your iframe automatically loads content from your site without prior 
notification and acceptance, you may in fact be violating tracking laws 
and our own guidelines. People have the right to opt-in to being tracked 
or monitored, and it's much harder to do that with an iframe that loads 
data right away, versus an API that can simply check 'Am I connected? No? 
Prompt for keys please!'

Iframes also make it difficult for people to tell if they're on their own 
site or yours, which can make them assume they've logged in to WordPress, 
not your service.

Also keep in mind that iframes are not always mobile friendly, and your 
plugin may become unusable on a phone or smaller. Finally, having a 
remote call like that, which loads an entire page without timeouts, can 
cause negative experiences with regards to usability, making a site hang 
and run slowly.

Related to this, we do not permit internal iframe usage (using an iframe 
on files included in your plugin) because it's a poor coding practice. 
PHP can natively include other files, and Javascript can ajaxify your 
interface if needed.

Example(s) from your plugin:

emailkit/includes/Admin/MetaBox.php:207: <iframe class="iframe-container" 
src="<?php echo EMAILKIT_URL . 'out/index.html' " 
frameborder="0"></iframe>
emailkit/includes/Admin/MetaField/SideMenu.php:44:<iframe 
class="iframe-container" src="<?php echo EMAILKIT_URL . 'out/index.html' 
" frameborder="0"></iframe>

## No publicly documented resource for your compressed content

In reviewing your plugin, we cannot find a non-compiled version of your 
javascript related source code.

In order to comply with our guidelines of human-readable code, we require 
you to include a link to the non-compressed, developer libraries you've 
included in your plugin. This may be in your source code, however we 
require you to also have it in your readme.

https://developer.wordpress.org/plugins/wordpress-org/detailed-plugin-guidelines/#4-code-must-be-mostly-human-readable

We strongly feel that one of the strengths of open source is the ability 
to review, observe, and adapt code. By maintaining a public directory of 
freely available code, we encourage and welcome future developers to 
engage with WordPress and push it forward.

That said, with the advent of larger and larger plugins using more 
complex libraries, people are making good use of build tools (such as 
composer or npm) to generate their distributed production code. In order 
to balance the need to keep plugin sizes smaller while still encouraging 
open source development, we require plugins to make the source code to 
any compressed files available to the public in an easy to find location, 
by documenting it in the readme.

For example, if you've made a Gutenberg plugin and used npm and webpack 
to compress and minify it, you must either include the source code within 
the published plugin or provide access to a public maintained source that 
can be reviewed, studied, and yes, forked.

We strongly recommend you include directions on the use of any build 
tools to encourage future developers.


emailkit/out/_next/static/chunks/polyfills-7b08e4c67f4f1b892f4b.js
emailkit/out/_next/static/chunks/framework-a62d654bd9699da79f2a.js
emailkit/out/_next/static/chunks/252-7e7cb32b13eb1e011e17.js


## Internationalization: Don't use variables or defines as text, context 
or text domain parameters.

Please do not use variable names for the text, context or text domain 
portion of a gettext function.

For example, this is a bad call: __( 'Translate me.' , $text_domain );

This is also wrong: __( 'There are '.$users.' online' , 'penfold-macrame' 
);

Also have in mind that if your plugin permalink is penfold-macrame, then 
your text domain should be the same.

The text domain also needs to be added to the plugin header. WordPress 
uses it to internationalize your plugin meta-data even when the plugin is 
disabled.

You can read 
https://developer.wordpress.org/plugins/internationalization/how-to-internationalize-your-plugin/#text-domains 
for more information.

Example(s) from your plugin:


emailkit/includes/Admin/Hooks.php:51 esc_html_e($status);



## Data Must be Sanitized, Escaped, and Validated

When you include POST/GET/REQUEST/FILE calls in your plugin, it's 
important to sanitize, validate, and escape them. The goal here is to 
prevent a user from accidentally sending trash data through the system, 
as well as protecting them from potential security issues.

SANITIZE: Data that is input (either by a user or automatically) must be 
sanitized as soon as possible. This lessens the possibility of XSS 
vulnerabilities and MITM attacks where posted data is subverted.

VALIDATE: All data should be validated, no matter what. Even when you 
sanitize, remember that you don't want someone putting in 'dog' when the 
only valid values are numbers.

ESCAPE: Data that is output must be escaped properly when it is echo'd, 
so it can't hijack admin screens. There are many esc_*() functions you 
can use to make sure you don't show people the wrong data.

To help you with this, WordPress comes with a number of sanitization and 
escaping functions. You can read about those here:

    https://developer.wordpress.org/apis/security/sanitizing/
    https://developer.wordpress.org/apis/security/escaping/


Remember: You must use the most appropriate functions for the context. If 
you're sanitizing email, use sanitize_email(), if you're outputting HTML, 
use wp_kses_post(), and so on.

An easy mantra here is this:

Sanitize early
Escape Late
Always Validate

Clean everything, check everything, escape everything, and never trust 
the users to always have input sane data. After all, users come from all 
walks of life.

Example(s) from your plugin:


emailkit/includes/Admin/MetaBox.php:258 $template_obj = 
$_POST['email_template_content_object'];
emailkit/includes/Admin/MetaBox.php:253 $template_html = 
$_POST['email_template_content_html'];
emailkit/includes/Admin/MetaBox.php:239 if 
(!isset($_POST['meta-box-nonce']) || 
!wp_verify_nonce($_POST['meta-box-nonce'], basename(__FILE__))) {
emailkit/includes/Admin/Api/UploadImage.php:39 $file = $_FILES['file'];
emailkit/includes/Admin/MetaBox.php:264 $type = 
$_POST["email_template_type"];
emailkit/includes/Admin/MetaBox.php:230 $post_id 
=$_POST['post_for_update'];



## Variables and options must be escaped when echo'd

Much related to sanitizing everything, all variables that are echoed need 
to be escaped when they're echoed, so it can't hijack users or (worse) 
admin screens. There are many esc_*() functions you can use to make sure 
you don't show people the wrong data, as well as some that will allow you 
to echo HTML safely.

At this time, we ask you escape all $-variables, options, and any sort of 
generated data when it is being echoed. That means you should not be 
escaping when you build a variable, but when you output it at the end. We 
call this 'escaping late.'

Besides protecting yourself from a possible XSS vulnerability, escaping 
late makes sure that you're keeping the future you safe. While today your 
code may be only outputted hardcoded content, that may not be true in the 
future. By taking the time to properly escape when you echo, you prevent 
a mistake in the future from becoming a critical security issue.

This remains true of options you've saved to the database. Even if you've 
properly sanitized when you saved, the tools for sanitizing and escaping 
aren't interchangeable. Sanitizing makes sure it's safe for processing 
and storing in the database. Escaping makes it safe to output.

Also keep in mind that sometimes a function is echoing when it should 
really be returning content instead. This is a common mistake when it 
comes to returning JSON encoded content. Very rarely is that actually 
something you should be echoing at all. Echoing is because it needs to be 
on the screen, read by a human. Returning (which is what you would do 
with an API) can be json encoded, though remember to sanitize when you 
save to that json object!

There are a number of options to secure all types of content (html, 
email, etc). Yes, even HTML needs to be properly escaped.

https://developer.wordpress.org/apis/security/escaping/

Remember: You must use the most appropriate functions for the context. 
There is pretty much an option for everything you could echo. Even 
echoing HTML safely.

Example(s) from your plugin:


emailkit/includes/Admin/MetaBox.php:207 <iframe src="<?php echo 
EMAILKIT_URL . 'out/index.html' ?>"  frameborder="0"></iframe>
 -----> echo EMAILKIT_URL . 'out/index.html';
emailkit/includes/Admin/MetaBox.php:85 <input type="hidden" 
name="post_for_update" id="post_for_update" value="<?php echo 
isset($object->ID)? $object->ID :  ''  ?>">
 -----> echo isset($object->ID) ? $object->ID : '';
emailkit/includes/Admin/MetaField/SideMenu.php:44 <iframe src="<?php echo 
EMAILKIT_URL . 'out/index.html' ?>"  frameborder="0"></iframe>
 -----> echo EMAILKIT_URL . 'out/index.html';
emailkit/EmailKit.php:92 window.emailKit.config = <?php echo 
json_encode($config); ?>;
emailkit/includes/Admin/Api/UploadImage.php:75 echo 
json_encode($response);



## Generic function/class/define/namespace/option names

All plugins must have unique function names, namespaces, defines, class 
and option names. This prevents your plugin from conflicting with other 
plugins or themes. We need you to update your plugin to use more unique 
and distinct names.

A good way to do this is with a prefix. For example, if your plugin is 
called "Easy Custom Post Types" then you could use names like these:

    function ecpt_save_post()
    define( 'ECPT_LICENSE', true );
    class ECPT_Admin{}
    namespace ECPT;
    update_option( 'ecpt_settings', $settings );


Don't try to use two (2) or three (3) letter prefixes anymore. We host 
nearly 100-thousand plugins on WordPress.org alone. There are tens of 
thousands more outside our servers. Believe us, you're going to run into 
conflicts.

You also need to avoid the use of __ (double underscores), wp_ , or _ 
(single underscore) as a prefix. Those are reserved for WordPress itself. 
You can use them inside your classes, but not as stand-alone function.

Please remember, if you're using _n() or __() for translation, that's 
fine. We're only talking about functions you've created for your plugin, 
not the core functions from WordPress. In fact, those core features are 
why you need to not use those prefixes in your own plugin! You don't want 
to break WordPress for your users.

Related to this, using if (!function_exists('NAME')) { around all your 
functions and classes sounds like a great idea until you realize the 
fatal flaw. If something else has a function with the same name and their 
code loads first, your plugin will break. Using if-exists should be 
reserved for shared libraries only.

Remember: Good prefix names are unique and distinct to your plugin. This 
will help you and the next person in debugging, as well as prevent 
conflicts.

Analysis result:


# This plugin is using the prefix "emailkit" for 15 element(s).

# Looks like there are 2 element(s) not using common prefixes.
emailkit/EmailKit.php:121 update_option('mk_installed', time());
emailkit/EmailKit.php:129 update_option('emilkit_version', 
EMAILKIT_VERSION);



----------------------------------------------

Please note that due to the significant backlog the Plugin Review team is 
facing, we have only done a basic review of your plugin. Once the issues 
we shared above are fixed, we will do a more in-depth review that might 
surface other issues. In order to prevent further delays, we strongly 
urge you to review the guidelines again before you resubmit it.

If the corrections we requested in this initial review are not completed 
within 3 months (90 days), we will reject this submission in order to 
keep our queue manageable and you will need to resubmit the plugin from 
scratch.

Your next steps are:

    Make all the corrections related to the issues we listed.
    Review your entire code following the guidelines to ensure there are 
no other related concerns.
    Attach your corrected plugin as a zip file OR provide a link to a 
public location (Dropbox, Github, etc) from where we can download the 
code. A direct link to the zip is best.
    Please do not send it using services where the download expires after 
a short period of time (such as WeTransfer-Free).

Once we receive your updated code, we will re-review it from top down.

Be aware that if your zip contains javascript files, you may not be able 
to email it as many hosts block that in the interests of security. Keep 
in mind, all version control directories (like Github) will auto-generate 
a zip for you, so you do not need to upload a zip file to their systems. 
You can just link to the repository.

We again remind you that should you wish to alter your permalink (not the 
display name, the plugin slug), you must explicitly tell us what you want 
it to be. We require to you clearly state in the body of your email what 
your desired permalink is. Permalinks cannot be altered after approval, 
and we generally do not accept requests to rename should you fail to 
inform us during the review.

If you previously asked for a permalink change and got a reply that is 
has been processed, you're all good! While these emails will still use 
the original display name, you don't need to panic. If you did not get a 
reply that we processed the permalink, let us know immediately. While we 
have tried to make this review as exhaustive as possible we, like you, 
are humans and may have missed things. As such, we will re-review the 
entire plugin when you send it back to us. We appreciate your patience 
and understanding.

If you have questions, concerns, or need clarification, please reply to 
this email and just ask us.


--
WordPress Plugin Review Team | plugins@wordpress.org
https://make.wordpress.org/plugins/
https://developer.wordpress.org/plugins/wordpress-org/detailed-plugin-guidelines/
