import { TeneoSDK, SecurePrivateKey } from "@teneo-protocol/sdk";

const sdk = new TeneoSDK({
  wsUrl: "wss://backend.developer.chatroom.teneo-protocol.ai/ws",
  privateKey: new SecurePrivateKey(process.env.PRIVATE_KEY!),
  autoSummon: true // Automatically add agents to rooms when needed
});

await sdk.connect();

// This is a paid command (0.001 USDC per request)
// Payment is handled automatically via autoApproveQuotes (enabled by default)
const response = await sdk.sendMessage("@__example__ /analyze ETH 24h", {
  room: "your-room-id",
  waitForResponse: true,
  timeout: 30000
});

console.log(response.content);

sdk.disconnect();

// If you prefer not to enable autoSummon globally, you can add agents manually:
//
// await sdk.agentRoom.addAgentToRoom("your-room-id", "__example__");
// const response = await sdk.sendMessage("@__example__ /analyze ETH 24h", { ... });


// Advanced: To inspect pricing before paying, disable auto-approve:
//
// const sdk = new TeneoSDK({ ..., autoApproveQuotes: false });
// const quote = await sdk.requestQuote("@__example__ /analyze ETH 24h", "your-room-id");
// console.log(`Price: ${quote.pricing.pricePerUnit} USDC`);
// const response = await sdk.confirmQuote(quote.taskId, { waitForResponse: true });
