> ## Documentation Index
> Fetch the complete documentation index at: https://docs.coingecko.com/llms.txt
> Use this file to discover all available pages before exploring further.

# cg.coin.info.updated

> Triggered when core coin information is updated on CoinGecko

Dispatched whenever a coin's metadata, links, categories, contract addresses, or images change on [CoinGecko.com](https://www.coingecko.com).

## Use Cases

> **Automated Asset Updates**
>
> Update your UI when a token rebrands, changes ticker, or updates its logo.

> **Cross-Chain Contract Monitoring**
>
> Detect when a project deploys on a new chain or migrates a contract address.

> **Risk & Compliance**
>
> Act on critical alerts — malicious activity warnings or contract migration notices.

> **Bots & Alert Systems**
>
> Notify your community when a project updates its whitepaper, GitHub repo, or social links.

## Tracked Fields

The webhook listens for changes in the following fields. Each change is reported as an `addition`, `update`, or `removal` in the payload.

| Field | Description | Example |
| - | - | - |
| `id` | CoinGecko coin ID. Typically immutable — only changes to fix errors. | `bitcoin`, `usd-coin` |
| `symbol` | Ticker symbol. | `btc`, `usdc` |
| `name` | Display name. | `Bitcoin`, `USDC` |
| `web_slug` | URL slug on CoinGecko.com. Can differ from `id`. | `bitcoin`, `usdc` |
| `platforms.{asset_platform_id}` | Token contract address on a given chain. See [Asset Platforms List](https://docs.coingecko.com/reference/asset-platforms-list.md). | `platforms.ethereum` → `0x1f98...` |
| `categories` | Category IDs assigned to the coin. | `meme-token`, `artificial-intelligence` |
| `public_notices` | Security breach info and contract migration announcements. | `$MATIC was upgraded 1:1 to $POL...` |
| `additional_notices.{status}` | Flags like `can_mint_new_tokens` or `has_unverified_contract`. | `true`, `false`, or `null` |
| `links.{site}` | Official websites and social media links. | `links.homepage`, `links.whitepaper`, `links.twitter_screen_name` |
| `image` | Image filename (not URL). Use [/coins/\{id}](https://docs.coingecko.com/reference/coins-id.md) or [/coins/markets](https://docs.coingecko.com/reference/coins-markets.md) to get full URLs. | `uniswap-logo.png` |

> [!NOTE]
>
> When `platforms.{asset_platform_id}` changes its key (e.g. `xdai` → `ethereum`), two changes are sent: a `removal` of the old key and an `addition` of the new key.

> [!NOTE]
>
> For `links.homepage`, `links.official_forum_url`, `links.chat_url`, and `links.announcement_url` — only `update` events are triggered.
>
> * The payload contains the full array of links.
> * A removed link returns an empty value.

## Payload

Each payload contains the event type, coin identity, and an array of `changes` describing exactly what was modified.

**event\_type** `string`

`cg.coin.info.updated`

**data** `object`

Coin and its changes.

**properties**

**id** `string`

CoinGecko coin ID

**symbol** `string`

Coin symbol

**name** `string`

Coin name

**changes** `object[]`

Array of change objects

**properties**

**field** `string`

Field that changed. See [Tracked Fields](#tracked-fields).

**change\_type** `string`

`addition`, `update`, or `removal`

**new\_value** `any`

Updated value. `null` if removal.

**old\_value** `any`

Previous value. `null` if addition.

### Example

```json expandable theme={null}
{
  "event_type": "cg.coin.info.updated",
  "data": {
    "id": "bitcoin",
    "symbol": "Fadk",
    "name": "fadaka",
    "changes": [
      {
        "field": "categories",
        "change_type": "addition",
        "old_value": null,
        "new_value": "Store of Value"
      },
      {
        "field": "links.facebook_username",
        "change_type": "update",
        "old_value": "bitcoins",
        "new_value": "fadaka"
      },
      {
        "field": "platforms.ethereum",
        "change_type": "removal",
        "old_value": "0x1f9840a85d5af5bf1d1762f925bdaddc4201f984",
        "new_value": null
      }
    ]
  }
}
```


This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.
