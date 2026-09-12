***name: steamwebapi
description: Build integrations with SteamWebAPI for Steam inventories, item catalogs, market prices and CS2 release discovery. Use when implementing or debugging a SteamWebAPI client or choosing its documented endpoints and data fields.
***
SteamWebAPI integrations

Use the current API contract to implement the user's requested integration. This
skill supplies integration guidance; it does not authorize account changes,
trades, purchases, deployments or other external writes.

Discover the contract

With the SteamWebAPI MCP connected, use search_endpoints, then get_endpoint
  for the selected route. Check required inputs, response schemas and examples
  before generating code. Use list_games for game identifiers and list_markets
  for current marketplace identifiers.
Without MCP, read the live OpenAPI document at
  https://www.steamwebapi.com/api/doc.json and documentation at
  https://www.steamwebapi.com/api/doc. Do not invent paths or parameters.
Treat live endpoint documentation as authoritative when it differs from this
  installed skill. Report unsupported capabilities explicitly.

Credentials and request behavior

Keep the SteamWebAPI API key on the backend in environment configuration.
  Do not embed it in browser bundles or print credential-bearing request URLs.
MCP discovery is available without a key. The optional
  call_readonly_endpoint tool uses the connection's configured credentials;
  it only executes allowed documented GET routes. Do not pass Steam cookies,
  Steam Guard secrets or full trade URLs into MCP tools.
Check HTTP status and parse errors. Handle rate limits with bounded backoff;
  do not retry indefinitely. Match refresh intervals and caching to the product's
  freshness requirements and endpoint quotas.
Use select and documented pagination for the required data. Do not silently
  return only the first page when the user needs a complete catalog or inventory.

Interpret item and price data

Use get_concept for price and identity fields before calculations. Steam
  listing prices, completed-sale prices and third-party prices are different
  observations. pricereal is a third-party market price, not a Steam sale.
Preserve unknown/null values. Do not turn unavailable prices into zero-value
  items or equate missing metadata with a confirmed negative fact.
An item catalog entry identifies a market item; it is not a unique inventory
  asset. Use the documented inventory identifiers for asset-specific operations.
variants on items describes phase variants, such as Doppler phases. It is
  not an enumeration of wear conditions or StatTrak/Souvenir versions. Variant
  pricereal may be null.

Discover newly released CS2 items

/steam/api/items?game=cs2&only_new_items=1 (also true) returns items missing
  from the normal database from all skin collections sharing the newest valid
  release date. It does not return every historical missing item.
with_preview_items=1 returns normal items plus those same missing items.
  Preview rows have preview: true, a normal-format item ID, collection name in
  tag7, and limited metadata. Do not assume all normal price fields exist on
  preview rows. select retains the preview marker automatically.
releasedat is the earliest known base-skin collection release date, or null.
  Its object shape is {"date":"2026-07-08 00:00:00.000000","timezone_type":3,"timezone":"UTC"};
  midnight is a presentation convention, not a known release time.
  It does not establish the release date of a specific Souvenir/StatTrak variant
  or its first Steam listing. Do not substitute it for a first-seen timestamp.
steamlisting on preview rows is inferred from a source Steam economy image;
  it is not a live listing-availability check. A preview inspectlink can be a
  generated visual certificate with representative wear, not a real owned asset.
Discovery depends on the upstream catalog and its release dates. An empty
  response is not proof that Valve has released no new content.

Verify the integration

Use a small representative read to verify fields, types, filters and pagination
before increasing volume. Use get_pricing only when quotas or plan selection
matter; size against current endpoint-group limits and the actual workload.
Distinguish verified API output from example data and untested assumptions.
