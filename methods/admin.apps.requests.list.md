List app requests for a team/workspace.

## Facts

| Method URL: | `https://slack.com/api/admin.apps.requests.list` |
| Preferred HTTP method: | `GET` |
| Accepted content types: | `application/x-www-form-urlencoded` |
| Rate limiting: | [Tier 2](/docs/rate-limits#tier_t2) |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [user](/docs/token-types#user) | [`admin.apps:read`](/scopes/admin.apps:read)&nbsp; |

 |

* * *

This [App Management API](/admins/managing) method lists pending app install requests for a workspace. It only lists requests for installation that have not yet been [approved](/methods/admin.apps.approve) or [restricted](/methods/admin.apps.restrict).

This method requires an `admin.*` scope. It's obtained through the normal [OAuth process](/docs/oauth), but there are a few additional requirements. The scope must be requested by an Enterprise Grid admin or owner, and the OAuth install must take place on the entire Grid org, not an individual workspace. See the [`admin.apps:read` page](/scopes/admin.apps:read) for more detailed instructions.

This [API method for admins](/enterprise/managing) may only be used on [Enterprise Grid](/enterprise).

## Arguments

 | Argument | Example | Required | Description |
| --- | --- | --- | --- |
 | `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `cursor` | `5c3e53d5` | Optional | Set `cursor` to `next_cursor` returned by the previous call to list items in the next page |
| `limit` | `100` | Optional, default=100 | The maximum number of items to return. Must be between 1 - 1000 both inclusive. |
| `team_id` | &nbsp; | Optional | |

<ts-icon class="ts_icon_code"></ts-icon>Present arguments as parameters in `application/x-www-form-urlencoded` querystring or POST body. This method does not currently accept `application/json`.

`team_id` is **required** if your Enterprise Grid org contains more than one workspace.

## Response

Typical success response

```
{
    "ok": true,
    "app_requests": [
        {
            "id": "Ar0XJGFLMLS",
            "app": {
                "id": "A061BL8RQ0",
                "name": "Test App",
                "description": "",
                "help_url": "",
                "privacy_policy_url": "https://testapp.com/privacy",
                "app_homepage_url": "",
                "app_directory_url": "https://acmecorp.slack.com/apps/A061BL8RQ0-test-app",
                "is_app_directory_approved": true,
                "is_internal": false,
                "icons": {
                    "image_32": "/cdn/157658203/img/testapp/service_32.png",
                    "image_36": "/cdn/157658203/img/testapp/service_36.png",
                    "image_48": "/cdn/157658203/img/testapp/service_48.png",
                    "image_64": "/cdn/157658203/img/testapp/service_64.png",
                    "image_72": "/cdn/157658203/img/testapp/service_72.png",
                    "image_96": "/cdn/157658203/img/testapp/service_96.png",
                    "image_128": "/cdn/157258203/img/testapp/service_128.png",
                    "image_192": "/cdn/157258203/img/testapp/service_192.png",
                    "image_512": "/cdn/15758203/img/testapp/service_512.png",
                    "image_1024": "/cdn/15258203/img/testapp/service_1024.png"
                },
                "additional_info": ""
            },
            "previous_resolution": null,
            "user": {
                "id": "W08RA9G5HR",
                "name": "Jane Doe",
                "email": "janedoe@example.com"
            },
            "team": {
                "id": "T0M94LNUCR",
                "name": "Acme Corp",
                "domain": "acmecorp"
            },
            "scopes": [
                {
                    "name": "incoming-webhook",
                    "description": "Post messages to specific channels in Slack",
                    "is_sensitive": false,
                    "token_type": "user"
                }
            ],
            "message": "test test again",
            "date_created": 1578956327
        }
    ],
    "response_metadata": {
        "next_cursor": ""
    }
}
```

Typical error response

```
{
    "ok": false,
    "error": "missing_scope",
    "needed": "admin.apps:read",
    "provided": "read,client,admin,identify,post,apps"
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `invalid_cursor` | Value passed for `cursor` was not valid or is no longer valid. |
| `feature_not_enabled` | Returned when the Admin APIs feature is not enabled for this team |
| `not_an_admin` | This method is only accessible by org owners and admins |
| `team_not_found` | Returned when team id is not found. |
| `app_management_app_not_installed_on_org` | The app management app must be installed on the org. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Some aspect of authentication cannot be validated. Either the provided token is invalid or the request originates from an IP address disallowed from making the request. |
| `account_inactive` | Authentication token is for a deleted user or workspace. |
| `token_revoked` | Authentication token is for a deleted user or workspace or the app has been removed. |
| `no_permission` | The workspace token used in this request does not have the permissions necessary to complete the request. Make sure your app is a member of the conversation it's attempting to post a message to. |
| `org_login_required` | The workspace is undergoing an enterprise migration and will not be available until migration is complete. |
| `ekm_access_denied` | Administrators have suspended the ability to post a message. |
| `missing_scope` | The token used is not granted the specific scope permissions required to complete this request. |
| `is_bot` | This method cannot be called by a bot user. |
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

