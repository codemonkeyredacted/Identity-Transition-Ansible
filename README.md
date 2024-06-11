# Identity-Transition-Ansible
Ensure you set the following variables:

1. Within inventory.ini input your IP for voting_validator (Current PROD Validator) and input your IP for hotspare_target (Current Hot Spare on Standby)
2. Within playbook.yml define the following variables (examples below):

Copy from Prod Validator to Temp Validator variables:

ledger_location: /ledger
staked_identity_location_folder: /home/sol
scp_username: sol
hotspare_ip: Use same hotspare IP from within inventory.ini
staked_identity_pubkey: {STAKEDPUBKEY}

Apply Transition to Temp Validator variables:

ledger_location: /ledger
staked_identity_location_folder: /home/sol
staked_identity_location: /home/sol/staked-identity.json

===========================================================================

"!!!Transition From Voting Validator to Hotspare" 
ansible-playbook -i ~/Identity-Transition-Ansible/inventory.ini ~/Identity-Transition-Ansible/Transition_From_Voting/playbook.yml

"!!!Transition From Hotspare Back to Voting Validator" 
ansible-playbook -i ~/Identity-Transition-Ansible/inventory.ini ~/Identity-Transition-Ansible/Transition_To_Voting/playbook.yml


Assumptions:

1. You have generated SSH keys for a SCP connection to be established
2. You have accepted the fingerprint of the host keys. I added -o StrictHostKeyChecking=no to my SCP command within the tasks. Feel free to remove that if you would like to manually accept the fingerprints. (Safer)
3. You have created a symlink for your staked identity or your staked identity is the main keypair for the validator. Ex: ln -sf /home/sol/{CurrentValidatorIdentity}.json /home/sol/staked-identity.json
4. You have changed your startup file to the following: 
  --identity ~/identity.json \
  --no-check-vote-account \
  --authorized-voter ~/staked-identity.json \
5. You have generated an unstaked identity by running the following:
solana-keygen new --outfile {keypair-path}/unstaked-identity.json
6. You have created a symlink for the identity.json by running ln -sf /home/sol/unstaked-identity.json /home/sol/identity.json
  
  
How can you contribute?

Please stake with my 0% Native/MEV Commission through Solana Beach! https://solanabeach.io/validator/5yHqB3NxovCEMUniQCboaPRMyyQ7kQQF4QqvC4vaz78z 