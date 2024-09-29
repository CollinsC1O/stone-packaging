# Stone Packaging

This project provides various forms of distribution for [Stone](https://github.com/starkware-libs/stone-prover) executables.

## About

The goal of this project is to reduce friction and speed up the process of generating proofs using Stone. More broadly, the aim is to make Stone a &#34;known&#34; piece of infrastructure that can be easily integrated into application-specific workflows and maintained efficiently.

## Roadmap

- [x] Static binary releases for x86_64
- [x] Static binary releases for ARM64
- [x] Minimal Docker images for x86_64
- [x] Native packages for Debian/Ubuntu

Follow-up work:
- Native packages for Fedora
- Native packages for Alpine
- Homebrew packages
- Technical documentation for file formats (inputs, outputs, memory, trace, proof), and test data
- Documentation hosted on GitHub Pages
- Integrated proof decomposition (related to https://github.com/zksecurity/stark-evm-adapter)
- Observability suite (metrics, dashboards, configurable logging)
- Stwo support

## Usage Instructions

### Download Binaries for x86_64

```bash
sudo wget https://github.com/dipdup-io/stone-packaging/releases/latest/download/cpu_air_prover-x86_64 -O /usr/local/bin/cpu_air_prover && sudo chmod +x /usr/local/bin/cpu_air_prover

sudo wget https://github.com/dipdup-io/stone-packaging/releases/latest/download/cpu_air_verifier-x86_64 -O /usr/local/bin/cpu_air_verifier && sudo chmod +x /usr/local/bin/cpu_air_verifier
```

### Download Binaries for macOS ARM64

```bash
wget https://github.com/dipdup-io/stone-packaging/releases/latest/download/cpu_air_prover-arm64 -O /usr/local/bin/cpu_air_prover && chmod +x /usr/local/bin/cpu_air_prover

wget https://github.com/dipdup-io/stone-packaging/releases/latest/download/cpu_air_verifier-arm64 -O /usr/local/bin/cpu_air_verifier && chmod +x /usr/local/bin/cpu_air_verifier
```

### Creating and Verifying a Test Proof Using Binaries

Clone the repository:

```bash
git clone https://github.com/dipdup-io/stone-packaging.git /tmp/stone-packaging
```

Navigate to the example test directory:

```bash
cd /tmp/stone-packaging/test_files/
```

Run the prover:
```bash
cpu_air_prover \
    --out_file=fibonacci_proof.json \
    --private_input_file=fibonacci_private_input.json \
    --public_input_file=fibonacci_public_input.json \
    --prover_config_file=cpu_air_prover_config.json \
    --parameter_file=cpu_air_params.json
```

The proof will be available at `fibonacci_proof.json`.

Run the verifier to verify the proof:

```bash
cpu_air_verifier --in_file=fibonacci_proof.json && echo "Successfully verified example proof."
```

## Download Minimal Docker Images for x86_64

Download the Docker image. The stone-prover package includes both cpu_air_prover and cpu_air_verifier:

```bash
docker pull ghcr.io/dipdup-io/stone-packaging/stone-prover:latest
```

### Creating and Verifying a Test Proof Using Docker


Run the Docker container with a volume mounted:

```bash
docker run --entrypoint /bin/bash -v /tmp/stone-packaging/test_files:/app/prover ghcr.io/dipdup-io/stone-packaging/stone-prover -c "cd /app/prover && exec cpu_air_prover \
    --out_file=fibonacci_proof.json \
    --private_input_file=fibonacci_private_input.json \
    --public_input_file=fibonacci_public_input.json \
    --prover_config_file=cpu_air_prover_config.json \
    --parameter_file=cpu_air_params.json"
```

The proof will be available at test_files/fibonacci_proof.json.

To verify the proof:

```bash
docker run --entrypoint /bin/bash -v /tmp/stone-packaging/test_files:/app/prover ghcr.io/dipdup-io/stone-packaging/stone-prover -c "cd /app/prover && exec cpu_air_verifier --in_file=fibonacci_proof.json && echo 'Successfully verified example proof.'"
```

## Download Native Packages for Debian/Ubuntu

Download the .deb package from the latest release:

```bash
wget https://github.com/dipdup-io/stone-packaging/releases/latest/download/stone-prover-linux-x86_64.deb && sudo dpkg -i stone-prover-linux-x86_64.deb
```

### Creating and Verifying a Test Proof Using the .deb Package

Navigate to the example test directory:

```bash
cd /tmp/stone-packaging/test_files/
```

Run the prover:
```bash
cpu_air_prover \
    --out_file=fibonacci_proof.json \
    --private_input_file=fibonacci_private_input.json \
    --public_input_file=fibonacci_public_input.json \
    --prover_config_file=cpu_air_prover_config.json \
    --parameter_file=cpu_air_params.json
```

The proof will be at `fibonacci_proof.json`.

Run the verifier to confirm the proof:
```bash
cpu_air_verifier --in_file=fibonacci_proof.json && echo "Successfully verified example proof."
```

This project is supported by Nethermind and Starknet Foundation via [OnlyDust platform](https://app.onlydust.com/p/stone-packaging-)
