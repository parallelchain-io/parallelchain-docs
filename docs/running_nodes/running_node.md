---
tags:
    - Running Nodes
---

# Running a Node

## Set up
---
Generate `config.toml` along with `keypair.json`, `genesis.json` and `config.toml`

>❗To join a network, make sure `genesis.json` is exactly the same as other members participating in the network.

*Example Config File*
```toml
   title = "Fullnode Configuration"

   standard_api_listening_port = 40000
   logs_path = "/tmp/parallelchain-f/logs"

   [engine]
   app_id = 0
   produce_block_time_limit = 5000
   validate_block_time_limit = 5000
   blocks_per_epoch = 8

   [engine.executor]
   contract_cache_folder = "/home/.parallelchain/fullnode/contract_cache"
   contract_memory_limit = 1073741824

   [engine.genesis]
   genesis_path = "/home/.parallelchain/fullnode/config/genesis.json"
   genesis_timestamp = 1672531200
   treasury_address = "HLW3Lch72b2m9snDwDF8pHgm_0mwzyTgnM_VtRRQfg4"

   [node]
   identity_path = "/home/.parallelchain/fullnode/config/keypair.json"
   block_tree_storage_path = "/home/.config/parallelchain/hotstuff_rs/block_tree_storage"

   [pacemaker]
   minimum_view_timeout = 10
   sync_request_limit = 10
   sync_response_timeout = 3

   [network]
   listening_port = 30000
   send_command_buffer_size = 8
   private_msg_buffer_size = 1024
   broadcast_msg_buffer_size = 1024
   peer_discovery_interval = 10

   # hotstuff_rs.static_participant_set represents the list of participants that belong to 
   # `hotstuff_rs::config::Configuration::static_participant_set`. In this example, only one participant
   # is available. Duplicate this entire segment [[hotstuff_rs.static_participant_set]] if you intend 
   # to add more participants.
   [[network.bootstrap]]
   address = "k9LhEzdX7SxssikrwNFMGiRfj4ugM3u3LNJvE0MY63U"
   ip_address = "172.16.3.1"
   port = 30000
   [[network.bootstrap]]
   address = "wT7l6TSj3h6Gg6xJQof-woz0qtYBrb-kLxWxO5q5sTw"
   ip_address = "172.16.3.2"
   port = 30000
   [[network.bootstrap]]
   address = "l1RjOEHtM-RvUh7BxmCBgu33Pw1vqb8AgKJLMLqz3js"
   ip_address = "172.16.3.3"
   port = 30000
   [[network.bootstrap]]
   address = "ipy_VXNiwHNP9mx6-nKxht_ZJNfYoMAcCnLykpq4x_k"
   ip_address = "172.16.3.4"
   port = 30000
```

!!!Tips
    -  Make sure keypair and ip pairs underneath network are consistent with the previous bootstrapped nodes present in the network. 
    - Add your own public key and ip_address in this section.
    - Make sure your keypair file is kept in the correct identity_path provided in `config.toml`.
  
## Run Binary
---
Run fullnode binary:

=== start from genesis
```bash
./fullnode --config-path /home/.parallelchain/fullnode/config/config.toml start-from-genesis
```

=== have existing storage
```bash
./fullnode --config-path /home/.parallelchain/fullnode/config/config.toml 
```