# QuantumBridge Protocol

[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Stacks](https://img.shields.io/badge/Built%20on-Stacks-purple.svg)](https://stacks.co)
[![Clarity](https://img.shields.io/badge/Language-Clarity-orange.svg)](https://clarity-lang.org)

## Overview

QuantumBridge is an advanced dual-party escrow protocol that revolutionizes peer-to-peer transactions on the Stacks blockchain. By implementing sophisticated state channels with cryptographic state management, QuantumBridge enables instantaneous value exchange between two parties while maintaining complete cryptographic integrity and security.

## Core Innovations

- **Zero-knowledge dispute resolution** with time-locked settlements
- **Atomic balance reconciliation** with multi-signature verification
- **Progressive funding mechanisms** for dynamic liquidity management
- **Cooperative and unilateral channel closure** pathways
- **Emergency recovery protocols** with governance safeguards

## Applications

- **High-frequency trading settlements** - Enable rapid transaction finalization
- **Streaming payment infrastructures** - Support continuous micropayments
- **Decentralized marketplace escrows** - Secure peer-to-peer commerce
- **Cross-chain liquidity bridging** - Facilitate inter-blockchain transfers

## System Architecture

### Contract Structure

```text
QuantumBridge Protocol
├── System Constants & Error Codes
├── Data Structures & Storage
├── Input Validation & Security Functions
├── Core Channel Management Functions
├── Channel Closure Mechanisms
└── Query & Administrative Functions
```

### Core Components

#### 1. **Payment Channels Data Model**

```clarity
{
  channel-id: (buff 32),        // Unique cryptographic identifier
  participant-a: principal,     // Channel initiator
  participant-b: principal,     // Channel counterparty
}
→
{
  total-deposited: uint,        // Total channel liquidity
  balance-a: uint,              // Participant A's balance
  balance-b: uint,              // Participant B's balance
  is-open: bool,                // Channel status
  dispute-deadline: uint,       // Challenge period deadline
  nonce: uint,                  // Replay protection counter
}
```

#### 2. **Security Validation Framework**

- Channel identifier format validation
- Deposit amount threshold enforcement
- Cryptographic signature verification
- Message construction standardization

#### 3. **Channel Lifecycle Management**

- **Creation**: Initialize channels with initial deposits
- **Funding**: Add additional liquidity to existing channels
- **Cooperative Closure**: Mutual consent settlement
- **Unilateral Closure**: Challenge-based dispute resolution

### Core Components

#### 1. **Payment Channels Data Model**

```clarity
{
  channel-id: (buff 32),        // Unique cryptographic identifier
  participant-a: principal,     // Channel initiator
  participant-b: principal,     // Channel counterparty
}
→
{
  total-deposited: uint,        // Total channel liquidity
  balance-a: uint,              // Participant A's balance
  balance-b: uint,              // Participant B's balance
  is-open: bool,                // Channel status
  dispute-deadline: uint,       // Challenge period deadline
  nonce: uint,                  // Replay protection counter
}
```

#### 2. **Security Validation Framework**

- Channel identifier format validation
- Deposit amount threshold enforcement
- Cryptographic signature verification
- Message construction standardization

#### 3. **Channel Lifecycle Management**

- **Creation**: Initialize channels with initial deposits
- **Funding**: Add additional liquidity to existing channels
- **Cooperative Closure**: Mutual consent settlement
- **Unilateral Closure**: Challenge-based dispute resolution

## Data Flow

### Channel Creation Flow

```text
1. Participant A initiates channel creation
2. Input validation (channel ID, deposit amount, counterparty)
3. Channel uniqueness verification
4. STX transfer to contract escrow
5. Channel state initialization
6. Channel becomes operational
```

### Cooperative Closure Flow

```text
1. Both parties agree on final balances
2. Dual signature verification
3. Balance conservation validation
4. Atomic fund distribution
5. Channel state finalization
```

### Unilateral Closure Flow

```text
1. Participant initiates unilateral closure
2. Dispute period activation (~7 days)
3. Challenge window for counterparty
4. Final settlement after timeout
5. Channel state cleanup
```

## API Reference

### Public Functions

#### `create-channel`

Creates a new payment channel between two participants.

**Parameters:**

- `channel-id` (buff 32): Unique channel identifier
- `participant-b` (principal): Counterparty address
- `initial-deposit` (uint): Initial STX deposit amount

**Returns:** `(ok true)` on success

#### `fund-channel`

Adds additional liquidity to an existing channel.

**Parameters:**

- `channel-id` (buff 32): Target channel identifier
- `participant-b` (principal): Counterparty address
- `additional-funds` (uint): Additional STX to deposit

**Returns:** `(ok true)` on success

#### `close-channel-cooperative`

Executes mutual channel closure with both parties' consent.

**Parameters:**

- `channel-id` (buff 32): Channel identifier
- `participant-b` (principal): Counterparty address
- `balance-a` (uint): Final balance for participant A
- `balance-b` (uint): Final balance for participant B
- `signature-a` (buff 65): Participant A's signature
- `signature-b` (buff 65): Participant B's signature

**Returns:** `(ok true)` on success

#### `initiate-unilateral-close`

Begins unilateral channel closure with dispute period.

**Parameters:**

- `channel-id` (buff 32): Channel identifier
- `participant-b` (principal): Counterparty address
- `proposed-balance-a` (uint): Proposed balance for participant A
- `proposed-balance-b` (uint): Proposed balance for participant B
- `signature` (buff 65): Initiator's signature

**Returns:** `(ok true)` on success

#### `resolve-unilateral-close`

Finalizes unilateral closure after dispute period expires.

**Parameters:**

- `channel-id` (buff 32): Channel identifier
- `participant-b` (principal): Counterparty address

**Returns:** `(ok true)` on success

### Read-Only Functions

#### `get-channel-info`

Retrieves comprehensive channel state information.

**Parameters:**

- `channel-id` (buff 32): Channel identifier
- `participant-a` (principal): Primary participant
- `participant-b` (principal): Secondary participant

**Returns:** Channel state object or `none`

## Error Codes

| Code | Constant | Description |
|------|----------|-------------|
| u100 | `ERR-NOT-AUTHORIZED` | Unauthorized access attempt |
| u101 | `ERR-CHANNEL-EXISTS` | Channel already exists |
| u102 | `ERR-CHANNEL-NOT-FOUND` | Channel does not exist |
| u103 | `ERR-INSUFFICIENT-FUNDS` | Insufficient funds for operation |
| u104 | `ERR-INVALID-SIGNATURE` | Invalid cryptographic signature |
| u105 | `ERR-CHANNEL-CLOSED` | Channel is already closed |
| u106 | `ERR-DISPUTE-PERIOD` | Invalid dispute period timing |
| u107 | `ERR-INVALID-INPUT` | Invalid input parameters |

## Security Considerations

### Cryptographic Security

- **Signature Verification**: All state transitions require cryptographic proofs
- **Replay Protection**: Nonce-based transaction ordering
- **Message Integrity**: Standardized message construction prevents tampering

### Economic Security

- **Balance Conservation**: Strict enforcement of fund conservation laws
- **Atomic Operations**: All fund transfers are atomic and reversible on failure
- **Dispute Resolution**: Time-locked challenge periods for fair resolution

### Access Control

- **Role-Based Permissions**: Participants can only act on their own channels
- **Emergency Controls**: Contract owner emergency withdrawal capabilities
- **Input Validation**: Comprehensive parameter validation on all functions

## Development Setup

### Prerequisites

- [Clarinet](https://github.com/hirosystems/clarinet) v1.0+
- [Node.js](https://nodejs.org/) v16+
- [VS Code](https://code.visualstudio.com/) with Clarity extension (recommended)

### Installation

```bash
# Clone the repository
git clone https://github.com/theo-lag/quantum-bridge.git
cd quantum-bridge

# Install dependencies
npm install

# Run contract checks
clarinet check

# Run tests
npm test
```

### Testing

```bash
# Run all tests
clarinet test

# Check contract syntax
clarinet check

# Deploy to testnet
clarinet deploy --testnet
```

## Contract Deployment

### Testnet Deployment

```bash
clarinet deploy --testnet
```

### Mainnet Deployment

```bash
clarinet deploy --mainnet
```

## Usage Examples

### Creating a Payment Channel

```javascript
// Using @stacks/transactions
import { makeContractCall } from '@stacks/transactions';

const txOptions = {
  contractAddress: 'SPXXXXX...', // Deployed contract address
  contractName: 'quantum-bridge',
  functionName: 'create-channel',
  functionArgs: [
    bufferCV(channelId),           // 32-byte channel ID
    principalCV(counterpartyAddr), // Counterparty address
    uintCV(1000000)               // 1 STX initial deposit
  ],
  // ... other transaction options
};

const transaction = await makeContractCall(txOptions);
```

### Cooperative Channel Closure

```javascript
const txOptions = {
  contractAddress: 'SPXXXXX...',
  contractName: 'quantum-bridge',
  functionName: 'close-channel-cooperative',
  functionArgs: [
    bufferCV(channelId),
    principalCV(counterpartyAddr),
    uintCV(finalBalanceA),
    uintCV(finalBalanceB),
    bufferCV(signatureA),
    bufferCV(signatureB)
  ]
};
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## Roadmap

- [ ] **v1.1**: Multi-party channel support
- [ ] **v1.2**: Cross-chain bridge integration
- [ ] **v1.3**: Governance token implementation
- [ ] **v2.0**: Layer 2 scaling solutions
