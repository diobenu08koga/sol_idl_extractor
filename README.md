# Solana IDL Extractor

A tool to extract Interface Description Language (IDL) from Solana programs through bytecode analysis and instruction parsing.

## Features

- Extracts instructions, accounts, and error codes from Solana programs
- Works with both Anchor and native Solana programs
- Advanced bytecode analysis techniques
- Instruction boundary identification
- Discriminator detection
- Symbol recovery and pattern-based naming

## Installation

```bash
cargo build --release
cargo install --path .
```

## Usage

```bash
solana-idl-extractor <PROGRAM_ID> [--output PATH] [--cluster URL] [--no-cache] [--simulate]
```

### Options

- `--output, -o PATH` - Save IDL to specified file path
- `--cluster, -c URL` - Use specified RPC URL (default: mainnet-beta)
- `--no-cache` - Don't use cached results
- `--simulate, -s` - Enhance IDL with transaction simulation
- `--clear-cache` - Clear cache for programs
- `--version, -v` - Show version information

### Examples

```bash
# Extract IDL for Token program
solana-idl-extractor TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA

# Save to file
solana-idl-extractor TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA --output token_idl.json

# Use different RPC endpoint
solana-idl-extractor TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA --cluster https://api.devnet.solana.com
```

## Library Usage

```rust
use solana_idl_extractor::{extract_idl, extract_idl_with_simulation};
use solana_pubkey::Pubkey;
use std::str::FromStr;

#[tokio::main]
async fn main() -> anyhow::Result<()> {
    let program_id = Pubkey::from_str("TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA")?;
    let rpc_url = "https://api.mainnet-beta.solana.com";
    
    let idl = extract_idl(&program_id, rpc_url, None, true).await?;
    
    Ok(())
}
```

## How It Works

1. **Bytecode Analysis**: Disassembles program ELF binary
2. **Discriminator Detection**: Identifies Anchor discriminators
3. **Transaction Analysis**: Analyzes historical transactions
4. **Simulation**: Optionally simulates transactions for accuracy
5. **Symbol Recovery**: Recovers meaningful names from patterns

## Limitations

- Not all instructions may be detected for complex programs
- Parameter types may be inferred incorrectly
- Account structure detection is best-effort
- Anchor programs are more accurately analyzed than native programs

## License

MIT License
