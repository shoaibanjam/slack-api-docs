Gets information about the current team.

## Facts

| Method URL: | `https://slack.com/api/team.info` |
| Preferred HTTP method: | `GET` |
| Accepted content types: | `application/x-www-form-urlencoded` |
| Rate limiting: | [Tier 3](/docs/rate-limits#tier_t3) |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [bot](/docs/token-types#granular_bot) | [`team:read`](/scopes/team:read)&nbsp; |
| [user](/docs/token-types#user) | [`team:read`](/scopes/team:read)&nbsp; |
| [classic&nbsp;bot](/docs/token-types#bot) | [`bot`](/scopes/bot) |

 |

* * *

This method provides information about your team.

## Arguments

 | Argument | Example | Required | Description |
| --- | --- | --- | --- |
 | `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `team` | &nbsp; | Optional | Team to get info on, if omitted, will return information about the current team. Will only return team that the authenticated token is allowed to see through external shared channels |

<ts-icon class="ts_icon_code"></ts-icon>Present arguments as parameters in `application/x-www-form-urlencoded` querystring or POST body. This method does not currently accept `application/json`.

## Response

Typical success response

```
{
    "ok": true,
    "team": {
        "id": "T12345",
        "name": "My Team",
        "domain": "example",
        "email_domain": "example.com",
        "icon": {
            "image_34": "https://...",
            "image_44": "https://...",
            "image_68": "https://...",
            "image_88": "https://...",
            "image_102": "https://...",
            "image_132": "https://...",
            "image_default": true
        },
        "enterprise_id": "E1234A12AB",
        "enterprise_name": "Umbrella Corporation"
    }
}
```

Typical error response

```
{
    "ok": false,
    "error": "invalid_auth"
}
```

## Team Icon

If a team has not yet set a custom icon, the value of `team.icon.image_default` will be `true`.

If the team belongs to an [Enterprise Grid](/enterprise-grid), the `enterprise_id` and `enterprise_name` fields will indicate the owning enterprise organization.

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Some aspect of authentication cannot be validated. Either the provided token is invalid or the request originates from an IP address disallowed from making the request. |
| `account_inactive` | Authentication token is for a deleted user or workspace. |
| `token_revoked` | Authentication token is for a deleted user or workspace or the app has been removed. |
| `no_permission` | The workspace token used in this request does not have the permissions necessary to complete the request. Make sure your app is a member of the conversation it's attempting to post a message to. |
| `org_login_required` | The workspace is undergoing an enterprise migration and will not be available until migration is complete. |
| `ekm_access_denied` | Administrators have suspended the ability to post a message. |
| `missing_scope` | The token used is not granted the specific scope permissions required to complete this request. |
| `invalid_arguments` | The method was called with invalid arguments. |
| `invalid_arg_name` | The method was passed an argument whose name falls outside the bounds of accepted or expected values. This includes very long names and names with non-alphanumeric characters other than `_`. If you get this error, it is typically an indication that you have made a _very_ malformed API call. |
| `invalid_charset` | The method was called via a `POST` request, but the `charset` specified in the `Content-Type` header was invalid. Valid charset names are: `utf-8` `iso-8859-1`. |
| `invalid_form_data` | The method was called via a `POST` request with `Content-Type` `application/x-www-form-urlencoded` or `multipart/form-data`, but the form data was either missing or syntactically invalid. |
| `invalid_post_type` | The method was called via a `POST` request, but the specified `Content-Type` was invalid. Valid types are: `application/json` `application/x-www-form-urlencoded` `multipart/form-data` `text/plain`. |
| `missing_post_type` | The method was called via a `POST` request and included a data payload, but the request did not include a `Content-Type` header. |
| `team_added_to_org` | The workspace associated with your request is currently undergoing migration to an Enterprise Organization. Web API and other platform operations will be intermittently unavailable until the transition is complete. |
| `request_timeout` | The method was called via a `POST` request, but the `POST` data was either missing or truncated. |
| `fatal_error` | The server could not complete your operation(s) without encountering a catastrophic error. It's possible some aspect of the operation succeeded before the error was raised. |

## Warnings

This table lists the expected warnings that this method will return. However, other warnings can be returned in the case where the service is experiencing unexpected trouble.

| Warning | Description |
| --- | --- |
| `missing_charset` | The method was called via a `POST` request, and recommended practice for the specified `Content-Type` is to include a `charset` parameter. However, no `charset` was present. Specifically, non-form-data content types (e.g. `text/plain`) are the ones for which `charset` is recommended. |
| `superfluous_charset` | The method was called via a `POST` request, and the specified `Content-Type` is not defined to understand the `charset` parameter. However, `charset` was in fact present. Specifically, form-data content types (e.g. `multipart/form-data`) are the ones for which `charset` is superfluous. |

