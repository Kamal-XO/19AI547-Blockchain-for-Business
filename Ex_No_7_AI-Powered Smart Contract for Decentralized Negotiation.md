# Experiment 7: AI-Powered Smart Contract for Decentralized Negotiation
# DATE : 30.04.2025
# Aim:
To create a smart contract that integrates AI logic for automated negotiation in decentralized commerce. The contract adjusts price and conditions dynamically based on real-time market trends using an on-chain AI model.

# Algorithm:
### Step 1:
Seller lists an item by specifying a base price, minimum price, and maximum price.

### Step 2: 
Buyer submits an offer with a proposed price and sends the equivalent funds.

### Step 3:
Smart contract logs the offer and checks if the item is still available.

### Step 4:
AI logic evaluates the offer using a dynamic pricing algorithm based on the offer, base, min, and max prices.

### Step 5:
If offer meets or exceeds AI counteroffer, the item is marked as sold and the seller receives payment.

### Step 6:
If offer is too low, the contract refunds the buyer and emits a counteroffer suggestion.

### Step 7:
Each successful sale updates pricing data, allowing smarter future counteroffers (simulated learning).

### Step 8:
Contract enables real-time, decentralized negotiation, imitating human bargaining through simple on-chain AI logic.
# Program:
```
NAME : RIYA P L
REG NO : 212223240141

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract AIPoweredNegotiation {
    struct Item {
        address seller;
        uint256 minPrice;
        uint256 maxPrice;
        uint256 basePrice;
        bool sold;
    }

    mapping(uint256 => Item) public items;
    uint256 public itemCount;

    event ItemListed(uint256 itemId, uint256 basePrice);
    event OfferMade(uint256 itemId, address buyer, uint256 offer);
    event CounterOffer(uint256 itemId, uint256 counterOffer);
    event Sold(uint256 itemId, address buyer, uint256 finalPrice);

    function listItem(uint256 _basePrice, uint256 _minPrice, uint256 _maxPrice) public {
        require(_minPrice <= _basePrice && _basePrice <= _maxPrice, "Invalid price range");
        
        items[itemCount] = Item(msg.sender, _minPrice, _maxPrice, _basePrice, false);
        emit ItemListed(itemCount, _basePrice);
        itemCount++;
    }

    function makeOffer(uint256 _itemId, uint256 _offerPrice) public payable {
        Item storage item = items[_itemId];
        require(!item.sold, "Item already sold");
        require(msg.value == _offerPrice, "Incorrect offer amount");

        emit OfferMade(_itemId, msg.sender, _offerPrice);

        uint256 aiCounterOffer = dynamicPricing(item.basePrice, item.minPrice, item.maxPrice, _offerPrice);

        if (_offerPrice >= aiCounterOffer) {
            item.sold = true;
            payable(item.seller).transfer(_offerPrice);
            emit Sold(_itemId, msg.sender, _offerPrice);
        } else {
            payable(msg.sender).transfer(_offerPrice); // Refund buyer
            emit CounterOffer(_itemId, aiCounterOffer);
        }
    }

    function dynamicPricing(uint256 base, uint256 min, uint256 max, uint256 offer) private pure returns (uint256) {
        if (offer >= max) return max;
        if (offer >= base) return base;
        uint256 counter =(base+offer)/2;
        return counter < min ? min : counter; // Enforce a Minimum bound
        
    }
}
```

# Output:
![Screenshot 2025-05-02 221227](https://github.com/user-attachments/assets/f6fc5074-ed30-429c-a5f5-31059e0cf8c4)

![Screenshot 2025-05-02 221256](https://github.com/user-attachments/assets/ff9ea95c-df63-415f-af7b-0a46a66d9064)

![Screenshot 2025-05-02 221318](https://github.com/user-attachments/assets/791aba2b-5929-410d-980e-65fd512a1311)

![Screenshot 2025-05-02 221348](https://github.com/user-attachments/assets/b3c6edd2-b5c6-4948-8cba-f2835a3f0539)

![Screenshot 2025-05-02 221410](https://github.com/user-attachments/assets/cffea713-ad78-4946-90db-7706694bf933)

# High-Level Overview:
First-of-its-kind AI-powered pricing contract.


Mimics real-world price negotiations using dynamic on-chain pricing.


Can be extended to AI oracles for real-time market data.


Inspired by AI-enhanced commerce and eBay-like decentralized auctions.

# RESULT:
Thus the AI-Powered Smart Contract for Decentralized Negotiation is implemented successfully.

