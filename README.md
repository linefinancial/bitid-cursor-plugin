# BitID MCP

Give AI agents verifiable identity and scoped authority. This plugin connects Cursor to
BitID's machine-agent layer so the agent can onboard decentralized identifiers (DIDs),
mint human-to-agent delegation credentials, and authorize individual actions with
zero-knowledge proofs — instead of handing an agent a long-lived API key.

## Install

1. Install this plugin from the Cursor Marketplace.
2. Go to **Cursor Settings → Plugins → BitID MCP → Configure**.
3. Paste your **service account key** (`bit_sk_…`). Create one in the BitID dashboard
   under *Service Accounts*.

The key is stored by Cursor and sent as the `x-service-account-key` header. It is never
committed to this repository.

## What you can do

Ask the agent things like:

- "Check that the BitID machine-agent services are healthy."
- "Onboard a human DID and an agent DID bound to it."
- "Mint a delegation credential for `pay.send` and `docs.read`."
- "Authorize a `pay.send` action for this agent and tell me why it was denied."
- "Show me how to integrate BitID into this React Native app."

## Tools

**Readiness**
| Tool | Purpose |
| --- | --- |
| `health_check` | `GET /health` for one or all five machine-agent services. |
| `preflight` | Fail-closed readiness check across all services plus the local gateway client. |

**Identity & delegation**
| Tool | Purpose |
| --- | --- |
| `onboard_human` | Allocate a human DID and register it in the Semaphore human group. |
| `onboard_agent` | Allocate a machine DID bound to a controller DID. |
| `issue_delegation` | Mint a human→agent delegation VC with explicit scopes. |
| `issue_sub_delegation` | Mint an agent→agent sub-delegation (scopes must be a subset of the parent). |
| `revoke_credential` | Revoke a credential by id. Destructive — requires `confirm=true`. |
| `credential_status` | Read a credential's revoked bit and the indexer revocation feed. |

**Authorization**
| Tool | Purpose |
| --- | --- |
| `prove_envelope` | Generate an AAE envelope with a fresh external nullifier. |
| `authorize_action` | Prove and authorize an action in one shot. |
| `nullifier_status` | Check whether a Semaphore nullifier has already been used. |

**Accounts & wallets**
| Tool | Purpose |
| --- | --- |
| `create_wallet` | Create platform wallets (BTC, ETH, CREATE2) for a user. |
| `generate_qr_code` | Create a BitID AuthID/QR session for display. |
| `verify_qr_scan` | Poll or approve a QR session. |

**Docs & verification**
| Tool | Purpose |
| --- | --- |
| `get_integration_docs` | BitID integration docs — start with `doc='catalog'`. |
| `run_quick_e2e` | Run the full onboarding → delegation → authorize E2E. |

## Resources

`ds-m2m://service-urls`, `ds-m2m://health`, `ds-m2m://troubleshooting`,
`ds-m2m://runtime`, `ds-m2m://docs/catalog`

## Prompts

`quick_e2e`, `onboard_agent`, `diagnose_deny`, `integrate_third_party`

## Requirements

- A BitID service account key.
- `preflight` and `run_quick_e2e` additionally need the `gateway-client` binary (or
  Cargo) available locally at `DS_M2M_ROOT`. Every other tool works over HTTP alone.

## Support

Issues: https://github.com/vishal-beem/bitid-cursor-plugin/issues

## License

MIT
