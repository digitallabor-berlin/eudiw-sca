# Strong-customer-authentication for the European Digital Identity Wallet
This repository contains a proposal on how the base protocols for the European Digital Identity Wallet (EUDIW) proposed by the [Architecture Reference Framework](https://github.com/eu-digital-identity-wallet/eudi-doc-architecture-and-reference-framework) of the European Commission might be used to perform strong-customer-authentication (SCA) for payments in the sense of the EU Revised Directive on Payment Services (PSD2).

> [!NOTE]
> DISCLAIMER: This publication is work-in-progress and will be progressively updated.

## Baseline Architecture proposed by the ARF
![Baseline Architecture](data-models-formats.png)


## Buildingblocks

```mermaid
block-beta
columns 1
  block:bank
    I["Request to Sign"]
    H["SCA with Request to Sign"]
    G["XS2A OpenBanking"]
  end
  block:creds
    F["OpenID4VCI"]
    E["OpenID4VP"]
    D["Presentation Exchange 2.0"]
  end
  block:rfc
    C["RFC 9101 JWT Secured Authorization Request"]
    B["RFC 9396 OAuth 2 Rich Authorization Request"]
  end
  block:oauth
    A["OAuth 2.0 Framework"]
  end
  classDef exist fill:#696,stroke:#333;
  classDef new fill:#ff0000,stroke:#333;
  class A,B,C,D,E,F,G exist
  class I,H new
```


## SCA and Payments using OpenID4VP

This paper describes how OpenID4VP might be used to perform SCA including dynamic linking according to the PSD2.

- [SCA based on OpenID4VP using OpenBanking](openbanking-r2s.md)

