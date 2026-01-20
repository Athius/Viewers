---
sidebar_position: 11
sidebar_label: AWS Cognito (OIDC)
title: AWS Cognito OpenID Connect Plan
summary: Step-by-step plan for integrating OHIF Viewer with AWS Cognito using OpenID Connect, before applying configuration values.
---

# AWS Cognito OpenID Connect Plan

This guide outlines a concrete plan to integrate the OHIF Viewer with AWS
Cognito using OpenID Connect (OIDC). It intentionally focuses on the **plan and
prerequisites** before applying the actual `oidc` configuration values inside
the Viewer.

## 1) Prepare Cognito

1. Create or choose an existing **User Pool**.
2. Create an **App Client** (public client for the viewer).
3. Configure a **Hosted UI domain** (Cognito domain or custom domain).
4. Set callback/sign-out URLs that match your Viewer deployment.
5. Ensure required scopes (at least `openid`, `email`, `profile`) are enabled.

## 2) Collect OIDC metadata

Gather the values that will be injected into the Viewer configuration later:

- **Issuer/Authority URL** (the user pool issuer URL)
- **Client ID**
- **Redirect URI** (Viewer callback route)
- **Post logout redirect URI**
- **Allowed scopes**

These values map directly to the OHIF Viewer `oidc` configuration keys, as
described in the Authorization and Authentication guide.

## 3) Decide on the authorization flow

The OHIF Viewer supports both the Implicit Flow and Authorization Code Flow.
For production setups, we recommend the **Authorization Code Flow**. When you
are ready to apply configuration, you will set:

- `response_type: 'code'`
- `useAuthorizationCodeFlow: true`

## 4) Plan token usage

The Viewer injects OIDC tokens into requests via the
`userAuthenticationService`. Verify that your DICOMweb or backend services
accept the `Authorization` header and that any reverse proxy (if used) passes
the header through.

## 5) Validate CORS and callback URLs

Make sure the Viewer origin is allowed in:

- Cognito app client callback URLs
- Any backend services the Viewer accesses (DICOMweb, metadata APIs, etc.)

## Next step

Once the plan above is complete and the required values are known, apply them
to the OHIF Viewer `oidc` configuration entry as described in the
Authorization and Authentication documentation.
