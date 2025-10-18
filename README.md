# Quantcode — quack demo

This repository contains experimental code that demonstrates:
- A simple BB84 quantum key-distribution simulation (Qiskit qasm_simulator).
- Fetching quantum-generated randomness from IBM Quantum backends (optional).
- A safe-ish deterministic key derivation example using HKDF-SHA256 that produces a 32-byte value mapped into the secp256k1 private-key range.
- An integrated demo script (quack) that ties the pieces together for demonstration and testing.

WARNING (critical)
- These scripts are educational demos only. Do NOT use keys or wallets created with these examples for storing real funds.
- The code intentionally avoids printing private keys by default, and the repository includes CI checks and tests to discourage committing secrets. Nevertheless, do not run the code on systems you do not fully control if you intend to generate sensitive keys.
- If you need production-quality key management, use audited libraries and a professionally-reviewed key-management process.

Quick start (demo-only)
1. Install dependencies (recommended into a virtualenv):
   python -m pip install --upgrade pip
   pip install qiskit qiskit-ibm-provider cryptography eth-account web3 pytest

2. Run the BB84 simulator:
   python quack --mode bb84 --key-size 128

3. Derive a private key from an entropy file via HKDF:
   python quack --mode hkdf --entropy-file entropy.bin

4. Run the integrated flow (BB84 -> HKDF -> Ethereum address):
   python quack --mode integrated --key-size 128

Notes about IBM Quantum
- To fetch randomness from IBM Quantum backends you must set the environment variable:
  export IBM_QUANTUM_TOKEN="YOUR_IBM_QUANTUM_API_TOKEN"
- Using real hardware involves queues, rate limits, and potential costs. The example scripts include simple timeouts and retries but are not production-ready.

Security and reproducibility
- HKDF derivation in this repo uses a random salt by default. If you need to reproduce a derived key you must store the salt securely.
- The code maps the HKDF output deterministically into the valid secp256k1 private-key range. This is a deliberate choice for demonstration; a production process should use established key-management standards.

CI and tests
- A GitHub Actions workflow is included under .github/workflows/ci.yml. The CI job installs dependencies and runs pytest.
- tests/test_no_private_key_prints.py scans the repo for likely 64-hex literals and "PRIVATE KEY" markers to reduce the risk of accidental commits of secret material.

Contributing & reporting issues
- See CONTRIBUTING.md for the repository's contribution guidelines and security policy.
- If you find a security issue, please do not open a public issue; follow the repository's security reporting process (or contact the maintainer directly).

License
- No license file is included in this demo. Add an appropriate open-source license before using this code publicly.
