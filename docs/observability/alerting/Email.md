# Email

Get status updates sent to your email inbox.

<img
  alt="Email"
  src="https://yourreclaimedlife.com/wp-content/uploads/2018/12/email-marketing-without-permission.jpg"
/>

## Configure email settings

In the root of your project’s folder (e.g. `default_repo/`), open the file `metadata.yaml`.

> Project folder name
>
> If you initialized Mage using a different project name, then your root folder will be named
differently. `default_repo` is the default project name if you didn’t customize it.

In the `default_repo/metadata.yaml` file, add a section with the following configuration:

```yaml
notification_config:
  email_config:
    smtp_host: ...
    smtp_mail_from: ...
    smtp_user: ...
    smtp_password: ...
    to_emails:
      - someone_lucky@mage.ai
      - eng@mage.ai
```

Change the values for each key under the `email_config` section.

<br />

## What next?

Whenever a pipeline run is successfully completed or fails,
an email will be delivered to all the inboxes listed under `to_emails`.

Here is an example of what an email could look like:

| |
| --- |
| Successfully ran Pipeline `example_pipeline` with Trigger 79 `hourly_trigger` at execution time `2022-09-28 19:00:00`.<br />Open http://localhost:6789/pipelines/example_pipeline/triggers/79 to check pipeline run results and logs. |

<br />
