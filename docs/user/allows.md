<h1>Allows</h1>

This form will give the user the ability to share resources with other roles, categories, groups and users

[TOC]

# Understanding permissions

There is a hierarchical classification of users based (from top to down):

1. ROLES
2. CATEGORIES
3. GROUPS
4. USERS

That classification will set the access to IsardVDI web application (roles) and also their [quota](quotas.md) (categories, groups and users). It will also set where the user resources will be stored in server:

​	`...<role>/<category>/<group>/<username>/...`

## Roles

The role defines mainly where those users will have access within the IsardVDI web application. There are items that can be managed:

- Desktops
- Templates
- Media

| ROLES         | USER ACCESS (own items) | ADMIN ACCESS (others items)      |
| ------------- | ----------------------- | -------------------------------- |
| Administrator | FULL                    | FULL                             |
| Manager       | FULL                    | FULL *ONLY IN HIS CATEGORY ITEMS |
| Advanced User | FULL                    | NONE                             |
| User          | Desktops only           | NONE                             |

## Categories ad Groups

These are only classification of users that will set them their [quotas](quotas.md) and limits. By default there is the **Default** category and group where all the new created users will be.

## Users

Of course there is the user. Different [quotas](quotas.md) can be set up for each user if needed.

# Allows Form

Users are allowed to share items  with others with this form. By default no access will be given to anyone (nothing is checked).

**NOTE**: From version 2.X resources shared inside a category are only seen in this category (or subgroups and users in that category). Only administrators can share items with categories. This was done to allow for multitenant set up.

- **NO ONE ALLOWED** (Default): Nothing is checked. That means that no role, category, group and user should be matched. Only the owner will get access.

![](../images/users/none_allowed.png)

- **EVERYONE ALLOWED**: If you check (and activate) any role, category, group or user check box but you don't add any in the search box beside, it will match with ANY existing.

![](../images/users/any_allowed.png)

- **SOME ALLOWED**: To allow only for a certain role, category, group o user, just check it's check box and search for it in the search box beside. Note that if you leave the search box empty everyone will get access to that template or media.

![](../images/users/some_allowed.png)
