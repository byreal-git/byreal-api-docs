# Router API Documentation

## Overview

The DEX router provides a swap endpoint for token swaps across DEX pools on Solana. The API supports both quote-only mode (price discovery) and transaction building mode (with user wallet).

## Endpoint

### POST `/byreal/api/router/v1/router-service/swap`

Performs token swap routing with optimal path finding across multiple DEX pools.

### Request

```json
{
  "inputMint": "string",           // Input token mint address (required)
  "outputMint": "string",          // Output token mint address (required)
  "amount": "string",              // Amount in lamports (required)
  "swapMode": "string",            // "in" or "out" (required)
  "slippageBps": "string",         // Slippage in basis points (required)
  "userPublicKey": "string",       // User wallet address (optional)
  "orderId": "string",             // External order ID for memo (optional)
  "broadcastMode": "string",       // "both", "priority", "jito" (optional)
  "feeType": "string",             // "maxCap" or "exactFee" (optional)
  "feeAmount": "string",           // Total fee in lamports (optional)
  "cuPrice": "string",             // CU price in microLamports/CU (optional)
  "jitoTip": "string",             // Jito tip in lamports (optional)
  "createInputAta": "boolean",     // Create input ATA if needed (optional, default: true)
  "createOutputAta": "boolean"     // Create output ATA if needed (optional, default: true)
  "excludeRouters": {
    "routers": ["AMM", "RFQ"]   // Routers to be excluded, e.g. if you only need the AMM quote, then you can exclude "RFQ" in routers (optional)
  }
}
```

#### Field Details

- **inputMint**: The mint address of the token being swapped from
- **outputMint**: The mint address of the token being swapped to
- **amount**: The amount to swap in lamports (smallest unit)
- **swapMode**:
    - `"in"`: Exact input amount (you specify input, get estimated output)
    - `"out"`: Exact output amount (you specify desired output, get required input)
- **slippageBps**: Slippage tolerance in basis points (1 bp = 0.01%)
- **userPublicKey**: If provided, returns a signed transaction; if not, returns quote only
- **orderId**: External order ID that will be included in transaction memo as JSON
- **broadcastMode**: Transaction broadcast strategy
    - `"both"`: Use both priority fees and Jito
    - `"priority"`: Use only Solana priority fees
    - `"jito"`: Use only Jito bundles
- **feeType**: Fee calculation mode
    - `"maxCap"`: Use provided fee as maximum cap
    - `"exactFee"`: Use exact fee amount
- **feeAmount**: Total fee budget in lamports
- **cuPrice**: Compute unit price for priority fees
- **jitoTip**: Tip amount for Jito bundles
- **createInputAta**: Whether to create Associated Token Account for input token
- **createOutputAta**: Whether to create Associated Token Account for output token
- **excludeRouters**: Which router to exclude, available values are "AMM", "RFQ", "JUPITER", "DFLOW". AMM stands for Byreal CLMM.

### Response

```json
{
  "result": {
    "retCode": 0,
    "retMsg": "Success"
  },
  "inputMint": "string",
  "outputMint": "string",
  "outAmount": "string",
  "otherAmountThreshold": "string",
  "priceImpactPct": 0.0,
  "inAmount": "string",
  "swapMode": "string",
  "transaction": "string",          // Base64 encoded transaction (if userPublicKey provided)
  "networkFee": "string",
  "poolAddresses": ["string"]
}
```

#### Example Response

```json
{
  "result": {
    "retCode": 0,
    "retMsg": "Quote Swap created successfully"
  },
  "inputMint": "5W84C59fbWLnzhM6ZQpBSPes6cm6dZnQ6czax89bRB2Y",
  "outputMint": "So11111111111111111111111111111111111111112",
  "outAmount": "50000000",
  "otherAmountThreshold": "34028984",
  "priceImpactPct": 21.403705196874412,
  "inAmount": "33859685",
  "swapMode": "out",
  "transaction": "AQAAAAAA...", // <- base64 encoded tx
  "networkFee": "0",
  "poolAddresses": [
    "J4jiEPEu8c8nLdpkiMa7k1P8rL1HCJSNxCvzA5DsmYds"
  ],
  "routerType": "AMM"
}
```

#### Response Fields

- **result**: Operation result with status code and message
- **inputMint**: Echo of input token mint
- **outputMint**: Echo of output token mint
- **outAmount**:
    - If `swapMode` is "in": Expected output amount
    - If `swapMode` is "out": Actual achievable output amount
- **otherAmountThreshold**: Minimum acceptable output (after slippage)
- **priceImpactPct**: Price impact percentage
- **inAmount**:
    - If `swapMode` is "out": Required input amount
    - If `swapMode` is "in": Same as request amount
- **swapMode**: Echo of swap mode
- **transaction**: Base64 encoded transaction (only if userPublicKey provided)
- **networkFee**: Network fee in lamports
- **poolAddresses**: List of pool addresses used in the route
- **routerType**: Indicates the returned quote type, available values are "AMM", "RFQ", "JUPITER", "DFLOW". AMM stands for Byreal CLMM.

### Error Responses

```json
{
  "result": {
    "retCode": 400,
    "retMsg": "Invalid input parameters"
  },
  "inputMint": "",
  "outputMint": "",
  "outAmount": "0",
  "otherAmountThreshold": "0",
  "priceImpactPct": 0.0,
  "inAmount": "0",
  "swapMode": "",
  "transaction": null,
  "networkFee": "0",
  "poolAddresses": []
}
```

Common error codes:
- `400`: Invalid request parameters
- `404`: Pool or route not found
- `500`: Internal server error

## Examples

### 1. Quote Only (Price Discovery)

```bash
curl -X POST https://api2.byreal.io/byreal/api/router/v1/router-service/swap \
  -H "Content-Type: application/json" \
  -d '{
    "inputMint": "So11111111111111111111111111111111111111112",
    "outputMint": "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v",
    "amount": "100000000",
    "swapMode": "in",
    "slippageBps": "50"
  }'
```

### 2. Build Transaction

```bash
curl -X POST https://api2.byreal.io/byreal/api/router/v1/router-service/swap \
  -H "Content-Type: application/json" \
  -d '{
    "inputMint": "So11111111111111111111111111111111111111112",
    "outputMint": "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v",
    "amount": "100000000",
    "swapMode": "in",
    "slippageBps": "50",
    "userPublicKey": "6wXkJRuNuKSoseyvb1nEF8QJr6AFewv5jeBp2A2XLT4a"
  }'
```

### 3. Exact Output Swap

```bash
curl -X POST https://api2.byreal.io/byreal/api/router/v1/router-service/swap \
  -H "Content-Type: application/json" \
  -d '{
    "inputMint": "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v",
    "outputMint": "So11111111111111111111111111111111111111112",
    "amount": "50000",
    "swapMode": "out",
    "slippageBps": "50",
    "userPublicKey": "89WMSTAPGVYTSBy861MpqvmrBCNiZ8Xntwvhw42xmExM"
  }'
```

### 4. With Fee Management

```bash
curl -X POST https://api2.byreal.io/byreal/api/router/v1/router-service/swap \
  -H "Content-Type: application/json" \
  -d '{
    "inputMint": "So11111111111111111111111111111111111111112",
    "outputMint": "EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v",
    "amount": "100000000",
    "swapMode": "in",
    "slippageBps": "50",
    "userPublicKey": "6wXkJRuNuKSoseyvb1nEF8QJr6AFewv5jeBp2A2XLT4a",
    "broadcastMode": "both",
    "feeType": "maxCap",
    "feeAmount": "500000",
    "cuPrice": "150000",
    "jitoTip": "100000"
  }'
```

## Fee Management

The router supports advanced fee management for transaction optimization:

### Broadcast Modes

1. **both**: Split fees between priority fees and Jito tips (50:50 for exactFee)
2. **priority**: Use entire fee budget for Solana priority fees
3. **jito**: Use entire fee budget for Jito tips

### Fee Types

1. **maxCap**: The `feeAmount` is a maximum cap. Actual fees may be lower based on provided `cuPrice` and `jitoTip`
2. **exactFee**: Use exactly the specified `feeAmount`, distributing based on broadcast mode

### Fee Distribution Logic

- For `both` mode with `exactFee`: Split 50:50 between priority and Jito
- For `both` mode with `maxCap`: Use provided values up to the cap
- For single mode (`priority` or `jito`): Use entire budget for that mode