# Blockchain Workshop
Blockchain is a decentralized and distributed digital ledger technology that allows multiple parties to maintain a shared record of transactions in a secure, transparent, and tamper-resistant manner. It was first introduced as the underlying technology behind Bitcoin, the first cryptocurrency, but its applications have since expanded to various industries and use cases beyond digital currencies.

In a blockchain, transactions are grouped into blocks, which are linked together in a chronological order to form a chain. Each block contains a unique identifier (hash) and a reference to the previous block, creating an immutable record of all the transactions. This structure ensures that any modifications to a previous block would require altering subsequent blocks, making it extremely difficult to tamper with the data stored on the blockchain.

Key features of blockchain technology include:
<ul>
<li>Decentralization: Instead of relying on a central authority or intermediary, blockchain operates on a peer-to-peer network where multiple participants (nodes) collectively validate and maintain the integrity of the ledger. This decentralization enhances transparency, eliminates single points of failure, and reduces the need for trust between parties.</li>

<li>Transparency: The blockchain ledger is publicly accessible, allowing anyone to view the transactions stored on it. This transparency promotes accountability and can be particularly useful in applications like supply chain management and financial auditing.</li>

<li>Security: Transactions on a blockchain are secured through cryptographic techniques. Each transaction is verified by the network participants and bundled into a block. Once added to the blockchain, the block is cryptographically sealed, making it extremely challenging for malicious actors to alter the data without detection.</li>

<li>Immutability: Once a block is added to the blockchain, it becomes nearly impossible to modify or delete the recorded transactions. This immutability ensures the integrity and trustworthiness of the data stored on the blockchain.</li>

<li>Blockchain technology has found applications in various fields, including finance, supply chain management, healthcare, voting systems, intellectual property, and more. It has the potential to streamline processes, reduce costs, enhance security, and enable new business models by eliminating the need for intermediaries and establishing trust among participants.</li></ul>

## Blockchain Components
### Block class
 Let's go through the different components of this class:

<b>Attributes:</b>
<ul>
<li><b>index</b>: It represents the position of the block in the blockchain.
<li><b>timestamp</b>: It stores the time at which the block is created.
<li><b>previousHash:</b> It holds the hash of the previous block in the blockchain, establishing the chronological order of blocks.
<li><b>currentHash:</b> It stores the hash of the current block.
<li><b>transactions:</b> It is a list of transactions included in the block.
<li><b>nonce:</b> It is a value used in the process of mining (proof-of-work) to find a valid hash for the block.
<li><b>Constructor:</b> The constructor initializes the attributes of the Block class when an instance is created. It takes parameters such as index, previousHash, transactions, and nonce. It also calculates the current hash of the block using the calculateHash() method.
<li><b>incrementNonce() method:</b> This method is used to increment the nonce value of the block. It is typically used in the mining process to find a valid hash by adjusting the nonce.
<li><b>calculateHash() method:</b> This method calculates the hash of the block using the block's attributes and a HashUtil class or method. In the provided code, an example implementation is shown where the hash is calculated by concatenating the index, timestamp, previousHash, transactions, and nonce, and then passing the concatenated data to HashUtil.calculateSHA256() to generate a SHA-256 hash.</li></ul>

It's worth noting that this code represents a simplified version of a blockchain block and does not include all the features and complexity found in a full-fledged blockchain implementation. Additional components such as validation, consensus mechanisms, and linking blocks together to form a chain would typically be part of a complete blockchain system.

```
import com.sdia.workShop.helper.HashUtil;
import lombok.Getter;
import lombok.Setter;
import java.time.Instant;
import java.util.List;
@Getter
@Setter
public class Block {

    private int index;

    private Instant timestamp;

    private String previousHash;

    private String currentHash;

    private List<Transaction> transactions;

    private int nonce;

    public Block(int index, String previousHash, List<Transaction> transactions, int nonce) {
        this.index = index;
        this.timestamp = Instant.now();
        this.previousHash = previousHash;
        this.transactions = transactions;
        this.nonce = nonce;
        this.currentHash = calculateHash();
    }

    public void incrementNonce() {
        nonce++;
    }

    public String calculateHash() {
        // Hash calculation logic using the block's attributes and HashUtil
        // Example implementation:
        String data = index + timestamp.toString() + previousHash + transactions.toString() + nonce;
        return HashUtil.calculateSHA256(data);
    }
}
```

### BlockChain class
Let's go through the different components of this class:

<b>Attributes:</b>
<ul>
<li><b>chain:</b> It is a list that holds the blocks in the blockchain.
<li><b>transactionPool:</b> It is an instance of the TransactionPool class, which manages pending transactions before they are included in a block.</li>
<li><b>difficulty: </b>It represents the difficulty level of mining, determining the number of leading zeros required in the block hash.</li></ul>
<b>Constructor:</b> The constructor initializes the attributes of the Blockchain class when an instance is created. It takes the difficulty parameter and sets up the chain and transaction pool. It also creates the genesis block (the first block in the blockchain) using the createGenesisBlock() method and adds it to the chain.</li>

<b>createGenesisBlock() method:</b> This method creates the genesis block, which is the first block in the blockchain. It creates an empty list of transactions and assigns index 0, previous hash "0", and nonce 0 to the block.</li>

<b>getLatestBlock() method:</b> This method returns the latest block in the chain by retrieving the block at the last index of the chain list.</li>

<b>addBlock(Block block) method:</b> This method is used to add a new block to the blockchain. It takes a Block object as a parameter. Before adding the block, it checks if the block is valid by calling the isValidBlock(Block block) method. If the block is valid, it adds the block to the chain, removes the block's transactions from the transaction pool, and returns the added block. Otherwise, it throws an InvalidParameterException.</li>

<b>isValidBlock(Block block) method: </b>This method verifies the validity of a block by comparing it with the previous block in the chain. It checks if the block's index is incremented by 1, if the previous hash matches the current hash of the previous block, and if the block's current hash meets the difficulty requirement (starts with a specific number of leading zeros).</li>

<b>mineBlock() method:</b> This method is responsible for mining a new block. It creates a new block with the necessary attributes, including the index, previous hash, pending transactions from the transaction pool, and nonce. It then iteratively increments the nonce and calculates the hash of the block until a valid hash is found (satisfying the difficulty requirement). Once a valid hash is found, the block is added to the blockchain by calling the addBlock(Block block) method.</li>

<b>getDifficultyPrefix(int difficulty) method:</b> This method returns a string of zeros that corresponds to the specified difficulty level. It is used to check the leading characters of a block's hash to meet the difficulty requirement.</li>

<b>validateChain() method:</b> This method validates the integrity of the entire blockchain by iterating through the blocks in the chain and checking if each block's current hash matches the calculated hash and if the previous hash matches the current hash of the previous block. If any inconsistency is found, it returns false. Otherwise, it returns true, indicating that the chain is valid.</li>

<b>addTransaction(Transaction transaction) method:</b> This method is used to add a new transaction to the transaction pool by calling the addTransaction(Transaction transaction) method of the TransactionPool class.</li></ul>

Overall, the provided code demonstrates the basic functionality of a blockchain system, including adding blocks, mining blocks, validating the chain, and managing transactions. However, it is important to note that this implementation may not include advanced features like consensus algorithms, peer-to-peer communication, or handling forks, which are commonly found in production

```
import lombok.Getter;
import lombok.Setter;

import java.security.InvalidParameterException;
import java.util.ArrayList;
import java.util.List;

@Getter
@Setter
public class Blockchain {

    private List<Block> chain;

    private TransactionPool transactionPool;

    private int difficulty;

    public Blockchain(int difficulty) {
        this.chain = new ArrayList<>();
        this.transactionPool = new TransactionPool();
        this.difficulty = difficulty;
        // Create the genesis block
        Block genesisBlock = createGenesisBlock();
        chain.add(genesisBlock);
    }

    private Block createGenesisBlock() {
        // Create the first block in the blockchain (genesis block)

        List<Transaction> transactions = new ArrayList<>(); // Empty list for genesis block

        return new Block(0, "0", transactions, 0);
    }

    public Block getLatestBlock() {
        return chain.get(chain.size() - 1);
    }

    public Block addBlock(Block block) {
        if (isValidBlock(block)) {
            chain.add(block);
            transactionPool.removeTransactions(block.getTransactions());
            return block;
        }
        throw new InvalidParameterException();
    }

    public boolean isValidBlock(Block block) {
        Block previousBlock = getLatestBlock();

        // Check if the index is incrementing by 1
        if (block.getIndex() != previousBlock.getIndex() + 1) {
            return false;
        }

        // Check if the previous hash matches
        if (!block.getPreviousHash().equals(previousBlock.getCurrentHash())) {
            return false;
        }

        // Check if the block hash meets the difficulty requirement
        String prefix = getDifficultyPrefix(difficulty);
        return block.getCurrentHash().startsWith(prefix);

    }

    public Block mineBlock() {

        Block block = new Block(getChain().size(), getLatestBlock().getCurrentHash(),
                transactionPool.getPendingTransactions(), 0);

        block.setPreviousHash(getLatestBlock().getCurrentHash());

        var calculatedHash = block.calculateHash();

        while (!calculatedHash.startsWith(getDifficultyPrefix(difficulty))) {
            block.incrementNonce();
            calculatedHash = block.calculateHash();
        }

        block.setCurrentHash(calculatedHash);

        return addBlock(block);
    }


    private String getDifficultyPrefix(int difficulty) {
        return "0".repeat(difficulty);
    }

    public boolean validateChain() {
        for (int i = 1; i < chain.size(); i++) {
            Block currentBlock = chain.get(i);
            Block previousBlock = chain.get(i - 1);

            // Check if the current hash of the block is valid
            if (!currentBlock.getCurrentHash().equals(currentBlock.calculateHash())) {
                return false;
            }

            // Check if the previous hash is equal to the hash of the previous block
            if (!currentBlock.getPreviousHash().equals(previousBlock.getCurrentHash())) {
                return false;
            }
        }
        return true;
    }

    public void addTransaction(Transaction transaction) {
        transactionPool.addTransaction(transaction);
    }
}
```
### Trasaction class
Let's go through the different components of this class:

<b>Attributes:</b>
<li><b>sender:</b> It represents the sender's address or identifier in the transaction.</li>
<li><b>recipient:</b> It represents the recipient's address or identifier in the transaction.</li>
<li><b>amount:</b> It indicates the amount of value being transferred in the transaction.</li>
<li><b>signature:</b> It is a placeholder attribute for the digital signature of the transaction.</li></ul>

<b>Constructor:</b> The constructor initializes the attributes of the Transaction class when an instance is created. It takes parameters such as sender, recipient, and amount. It assigns these values to the respective attributes and sets an initial empty string for the signature.</li>

<b>toString() method:</b> This method provides a string representation of the Transaction object. It returns a formatted string that includes the values of the sender, recipient, amount, and signature attributes.</li></ul>

The Transaction class represents a basic structure for a transaction in a blockchain system. In a real-world implementation, additional methods and logic would typically be included to handle aspects like verifying signatures, transaction validation, and handling transaction inputs and outputs. Furthermore, a complete blockchain system would typically have additional features and classes to manage transactions and their inclusion in blocks.

```
import lombok.Getter;
import lombok.Setter;

@Getter
@Setter
public class Transaction {

    private final String sender;

    private final String recipient;

    private final double amount;

    private String signature;

    public Transaction(String sender, String recipient, double amount) {
        this.sender = sender;
        this.recipient = recipient;
        this.amount = amount;
        this.signature = ""; // Placeholder for digital signature
    }

    @Override
    public String toString() {
        return "Transaction{" +
                "sender='" + sender + '\'' +
                ", recipient='" + recipient + '\'' +
                ", amount=" + amount +
                ", signature='" + signature + '\'' +
                '}';
    }

}

```
### TransactionPool Class
Let's go through the different components of this class:

<b>Attributes:</b>

<ul><li><b>pendingTransactions: </b> It is a list that holds the pending transactions waiting to be included in a block.</li></ul>
<b>Constructor:</b> The constructor initializes the TransactionPool class by creating an empty ArrayList to store the pending transactions.

<b>addTransaction(Transaction transaction) method:</b> This method is used to add a new transaction to the transaction pool. It takes a Transaction object as a parameter and adds it to the pendingTransactions list.

<b>getPendingTransactions() method:</b> This method returns the list of pending transactions stored in the transaction pool.

<b>removeTransaction(Transaction transaction) method:</b> This method removes a specific transaction from the transaction pool. It takes a Transaction object as a parameter and removes it from the pendingTransactions list.

<b>removeTransactions(List<Transaction> transactions) method:</b> This method removes multiple transactions from the transaction pool. It takes a list of Transaction objects as a parameter and removes all the transactions in the list from the pendingTransactions list.

The TransactionPool class provides basic functionality for adding, retrieving, and removing transactions from the pool. In a complete blockchain implementation, additional features may be added, such as transaction validation, transaction prioritization, and transaction expiration. Furthermore, other classes and methods would handle the processing of transactions from the pool, including selecting transactions to include in a block, verifying signatures, and updating balances or other relevant data structures.

```
import lombok.Getter;
import lombok.Setter;

import java.util.ArrayList;
import java.util.List;

@Getter
@Setter
public class TransactionPool {

    private final List<Transaction> pendingTransactions;

    public TransactionPool() {
        this.pendingTransactions = new ArrayList<>();
    }

    public void addTransaction(Transaction transaction) {
        pendingTransactions.add(transaction);
    }

    public List<Transaction> getPendingTransactions() {
        return pendingTransactions;
    }

    public void removeTransaction(Transaction transaction) {
        pendingTransactions.remove(transaction);
    }

    public void removeTransactions(List<Transaction> transactions) {
        pendingTransactions.removeAll(transactions);
    }

}
```

### HashUtil class 
The HashUtil class is a utility class that encapsulates the functionality of calculating the SHA-256 hash for a given data string. The SHA-256 (Secure Hash Algorithm 256-bit) is a widely used cryptographic hash function that produces a fixed-size 256-bit hash value. It is commonly used in blockchain technology for ensuring data integrity and security.
```
import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;

public class HashUtil {

    private HashUtil(){throw new IllegalAccessError("Invalid call to constructor");}
    public static String calculateSHA256(String data) {
        try {
            MessageDigest digest = MessageDigest.getInstance("SHA-256");
            byte[] hash = digest.digest(data.getBytes());

            StringBuilder hexString = new StringBuilder();
            for (byte b : hash) {
                String hex = Integer.toHexString(0xff & b);
                if (hex.length() == 1) {
                    hexString.append('0');
                }
                hexString.append(hex);
            }
            return hexString.toString();
        } catch (NoSuchAlgorithmException e) {
            e.printStackTrace();
        }
        return null;
    }
}

```

### Blockchaine Controller
The BlockchaineController class provides an API interface for interacting with the blockchain, including retrieving the blockchain, adding transactions, mining blocks, retrieving specific blocks, retrieving pending transactions, and validating the blockchain. These endpoints can be used by external clients to interact with the blockchain system.

Let's go through the different components of the code:

<b>Attributes:</b>

<ul><li><b>blockchain:</b> It is an instance of the Blockchain class that the controller interacts with.</li></ul>
<b>getBlockchain() method:</b> This method is mapped to the GET request at "/blockchain" endpoint. It retrieves the entire blockchain by calling the getChain() method of the blockchain instance and returns it as a response.

<b>addTransaction(Transaction transaction) method:</b> This method is mapped to the POST request at "/blockchain/transaction" endpoint. It takes a Transaction object in the request body, adds it to the blockchain by calling the addTransaction() method of the blockchain instance, and returns a ResponseEntity with a success message.

<b>mineBlock() method:</b> This method is mapped to the POST request at "/blockchain/mine" endpoint. It mines a new block by calling the mineBlock() method of the blockchain instance. The mined block is stored in the newBlock variable, and a ResponseEntity is returned with a success message along with the hash of the mined block.

<b>getBlockByIndex(int index) method:</b> This method is mapped to the GET request at "/blockchain/block/{index}" endpoint, where {index} is a path variable representing the index of the desired block. It retrieves the block at the specified index from the blockchain by calling the getChain() method of the blockchain instance, and returns a ResponseEntity with the retrieved block if it exists. Otherwise, it returns a ResponseEntity with a "Not Found" status.

<b>getTransactionPool() method:</b> This method is mapped to the GET request at "/blockchain/transaction-pool" endpoint. It retrieves the pending transactions from the transaction pool of the blockchain by calling the getPendingTransactions() method of the transaction pool, and returns the list of pending transactions as a response.

<b>validateChain() method:</b> This method is mapped to the GET request at "/blockchain/validate" endpoint. It validates the integrity of the blockchain by calling the validateChain() method of the blockchain instance. If the blockchain is valid, it returns a ResponseEntity with a success message. Otherwise, it returns a ResponseEntity with an "Internal Server Error" status and an error message indicating that the blockchain is invalid.

```
import lombok.RequiredArgsConstructor;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequiredArgsConstructor
public class BlockchaineController {
    private final Blockchain blockchain;

    @GetMapping("/blockchain")
    public List<Block> getBlockchain() {
        return blockchain.getChain();
    }

    @PostMapping("/blockchain/transaction")
    public ResponseEntity<String> addTransaction(@RequestBody Transaction transaction) {
        blockchain.addTransaction(transaction);
        return ResponseEntity.ok("Transaction added successfully.");
    }

    @PostMapping("/blockchain/mine")
    public ResponseEntity<String> mineBlock() {
        Block newBlock = blockchain.mineBlock();
        return ResponseEntity.ok("Block mined successfully. Block hash: " + newBlock.getCurrentHash());
    }

    @GetMapping("/blockchain/block/{index}")
    public ResponseEntity<Block> getBlockByIndex(@PathVariable int index) {
        if (index >= 0 && index < blockchain.getChain().size()) {
            Block block = blockchain.getChain().get(index);
            return ResponseEntity.ok(block);
        } else {
            return ResponseEntity.notFound().build();
        }
    }

    @GetMapping("/blockchain/transaction-pool")
    public List<Transaction> getTransactionPool() {
        return blockchain.getTransactionPool().getPendingTransactions();
    }

    @GetMapping("/blockchain/validate")
    public ResponseEntity<String> validateChain() {
        boolean isValid = blockchain.validateChain();
        if (isValid) {
            return ResponseEntity.ok("Blockchain is valid.");
        } else {
            return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body("Blockchain is invalid.");
        }
    }
}

```
### Asymmetric Encryption 
The AsymmetricEncryption class encapsulates the functionality of generating a key pair and performing encryption and decryption using the RSA algorithm. This type of encryption is asymmetric, meaning it uses different keys for encryption and decryption. The public key is used for encryption, while the private key is used for decryption. It provides a secure way to encrypt and decrypt data in scenarios where confidentiality and integrity are required.
```
import javax.crypto.Cipher;
import java.security.KeyPair;
import java.security.KeyPairGenerator;
import java.security.PrivateKey;
import java.security.PublicKey;

public class AsymmetricEncryption {

    private AsymmetricEncryption() {
    }

    public static KeyPair generateKeyPair() throws Exception {
        KeyPairGenerator keyPairGenerator = KeyPairGenerator.getInstance("RSA");
        keyPairGenerator.initialize(2048);
        return keyPairGenerator.generateKeyPair();
    }

    public static byte[] encrypt(byte[] plainText, PublicKey publicKey) throws Exception {
        Cipher cipher = Cipher.getInstance("RSA");
        cipher.init(Cipher.ENCRYPT_MODE, publicKey);
        return cipher.doFinal(plainText);
    }

    public static byte[] decrypt(byte[] encryptedText, PrivateKey privateKey) throws Exception {
        Cipher cipher = Cipher.getInstance("RSA");
        cipher.init(Cipher.DECRYPT_MODE, privateKey);
        return cipher.doFinal(encryptedText);
    }
}

```

### Hash Function 
```
public class HashFunction {

    public static byte[] hash(String data) throws NoSuchAlgorithmException {
        MessageDigest md = MessageDigest.getInstance("SHA-256");
        md.update(data.getBytes());
        return md.digest();
    }
}
```
### Symmetric Encryption
The SymmetricEncryption class encapsulates the functionality of generating a symmetric encryption key and performing encryption and decryption using the AES algorithm. Symmetric encryption uses the same key for both encryption and decryption. It provides a fast and efficient way to encrypt and decrypt data, making it suitable for scenarios where both the sender and the receiver share the same key and require a secure communication channel.

```
public class SymmetricEncryption {

    private SymmetricEncryption() {
    }

    public static Key generateKey() throws Exception {
        KeyGenerator keyGenerator = KeyGenerator.getInstance("AES");
        SecureRandom secureRandom = new SecureRandom();
        keyGenerator.init(256, secureRandom);
        return keyGenerator.generateKey();
    }

    public static byte[] encrypt(byte[] plainText, Key key) throws Exception {
        Cipher cipher = Cipher.getInstance("AES");
        cipher.init(Cipher.ENCRYPT_MODE, key);
        return cipher.doFinal(plainText);
    }

    public static byte[] decrypt(byte[] encryptedText, Key key) throws Exception {
        Cipher cipher = Cipher.getInstance("AES");
        cipher.init(Cipher.DECRYPT_MODE, key);
        return cipher.doFinal(encryptedText);
    }

```

