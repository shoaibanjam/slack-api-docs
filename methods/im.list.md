Lists direct message channels for the calling user.

## Facts

| Method URL: | `https://slack.com/api/im.list` |
| Preferred HTTP method: | `GET` |
| Accepted content types: | `application/x-www-form-urlencoded` |
| Rate limiting: | [Tier 2](/docs/rate-limits#tier_t2) |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [bot](/docs/token-types#granular_bot) | [`im:read`](/scopes/im:read)&nbsp; |
| [user](/docs/token-types#user) | [`im:read`](/scopes/im:read)&nbsp; |
| [classic&nbsp;bot](/docs/token-types#bot) | [`bot`](/scopes/bot) |

 |

* * *

<ts-icon class="ts_icon_warning"></ts-icon>
**This method is deprecated.** [&nbsp;Learn more](/changelog/2020-01-deprecating-antecedents-to-the-conversations-api).

Please use these methods instead:

- [`conversations.list`](/methods/conversations.list)
- [`users.conversations`](/methods/users.conversations)

Don't use this method. Use [`conversations.list`](/methods/conversations.list) instead.

This legacy method returns a list of all direct message channels the user has.

## Arguments

 | Argument | Example | Required | Description |
| --- | --- | --- | --- |
 | `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `cursor` | `dXNlcjpVMDYxTkZUVDI=` | Optional | Paginate through collections of data by setting the `cursor` parameter to a `next_cursor` attribute returned by a previous request's `response_metadata`. Default value fetches the first "page" of the collection. See [pagination](/docs/pagination) for more detail. |
| `limit` | `20` | Optional | The maximum number of items to return. Fewer than the requested number of items may be returned, even if the end of the users list hasn't been reached. |

<ts-icon class="ts_icon_code"></ts-icon>Present arguments as parameters in `application/x-www-form-urlencoded` querystring or POST body. This method does not currently accept `application/json`.

## Response

Returns a list of [IM objects](/types/im):

Typical success response

```
{
    "ok": true,
    "ims": [
        {
            "id": "D0G9QPY56",
            "created": 1449709280,
            "is_im": true,
            "is_org_shared": false,
            "user": "USLACKBOT",
            "is_user_deleted": false
        },
        {
            "id": "D1KL59A72",
            "created": 1466692204,
            "is_im": true,
            "is_org_shared": false,
            "user": "U0G9QF9C6",
            "is_user_deleted": false
        },
        {
            "id": "D0G9XPFH9",
            "created": 1449722883,
            "is_im": true,
            "is_org_shared": false,
            "user": "U0G9WFXNZ",
            "is_user_deleted": false
        },
        {
            "id": "D0HRHJSF7",
            "created": 1452098023,
            "is_im": true,
            "is_org_shared": false,
            "user": "W0HRJL7CK",
            "is_user_deleted": false
        },
        {
            "id": "D1GD7CHC0",
            "created": 1465834222,
            "is_im": true,
            "is_org_shared": false,
            "user": "U1GDBDGR3",
            "is_user_deleted": true
        },
        {
            "id": "D1QMF76M9",
            "created": 1468274703,
            "is_im": true,
            "is_org_shared": false,
            "user": "U1QNSQB9U",
            "is_user_deleted": false
        },
        {
            "id": "D6K48KKRN",
            "created": 1502210225,
            "is_im": true,
            "is_org_shared": false,
            "user": "U6KR7BVFW",
            "is_user_deleted": false
        }
    ],
    "response_metadata": {
        "next_cursor": "aW1faWQ6RDBCSDk1RExI="
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

## Pagination
This method uses cursor-based pagination to make it easier to incrementally collect information. To begin pagination, specify a `limit` value under `1000`. We recommend no more than `200` results at a time.  
  
Responses will include a top-level `response_metadata` attribute containing a `next_cursor` value. By using this value as a `cursor` parameter in a subsequent request, along with `limit`, you may navigate through the collection page by virtual page.  
  
 See [pagination](/docs/pagination) for more information.  
  

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `invalid_cursor` | Value passed for `cursor` was not valid or is no longer valid. |
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

