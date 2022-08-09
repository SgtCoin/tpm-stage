# Filecoin Core Devs Meeting #48 - 2022-08-04

**Meeting Date & Time:** Thursday, August 4, 2022 @ [16:00 UTC](https://www.timeanddate.com/worldclock/converter.html?iso=20210903T180000&p1=1440)

**Meeting Recording:** 

**Meeting Slides:** https://docs.google.com/presentation/d/1W8aan93-IVOfWPUPeFw4qh1g3jhqIXpRFpwXK0dK6qk/edit?usp=sharing

**Attendance:** I have a list of names, but not sure which team is who. Should I include the list of names?

### New Meeting Format

#### Welcome
* Shift away from standard and ecosystem work
* Fewer meetings; higher attendance is the goal
* Host more technical discussions
* Make collective governance decisions
#### Expectations, questions, and future planning
* Core devs are asked to
  * Propose discussion topics
  * Attend meetings
  * Join #fil-core-devs on slack
  * Participate in decision-making
    * Approval of FRCs
    * Confirmations of network upgrades.
    * Be a contributing factor to governance decisions.
            
### Engineering Discussion[TENT]: Standard Message Authentication Method for Actors
* Led by @Aayush Rajasekaran
* [Pull Request 424](https://github.com/filecoin-project/FIPs/pull/424)
* Conceptionally, the change is very small. The motivation is around future work. In one line, the proposal is to allow actors to authenticate or authorize a piece of data. An example of this today is account actors can service storage client. Part of that is finding a deal proposal. When the storage market actor verifies that they account actor is authorizing this deal proposal, they simply perform a signature validation. This means that only account actors can serve as storage clients.
* [FIP-0035](https://github.com/filecoin-project/FIPs/blob/master/FIPS/fip-0035) overlaps here. This process much simpler to allow actors to adopt a standard for how they can authorize data. Basically we are going to add authorized method to the account actor.
* With this model, we could allow actors to authorize signatures vs just confirming signatures.
* Concrete proposal for NV17 with just a few lines of code.
* Aayush is interested in feedback on this. Some overlap between this discussion and the broader conversation around account abstractions, which allows for any actors to serve as top level message centers.
* Alex N proposed FIP-0035.
  * He will withdraw the FIP if we move forward with this. He believes this is a far better way to do it.
### Engineering Discussion: Signature Domain Separation
* Led by Geoff Stuart
* [Pull Request 423](https://github.com/filecoin-project/FIPs/pull/4230)
* All signed data on the network are cbor encoded byte arrays. The problem is that if I am signing just a array of bytes, that could be de-serialized into any data type. Theoretically, I could sign a message, send it on chain and someone else could de-serialize the message into a invalid block header and submit it to the chain. I would get slashed for sending that message to the chain.
* This isnt how it works right now due to cbor encoding. However, FVM will allow users to sign arbitrary data and put it on chain.
* We want to put a fix length tag in front of any data where the tag is unique to the type of data signed. The tag would be prepended to your original data and we would send the original data to the chain. Anyone else would be able to infer the tag based on the type of data.
* Implementing this would be a two step process. Some messages, signed before the upgrade, would still be allowed and valid. We would also need time to get the wallet teams onboard with the new signature team.
  #### Questions
     * What do we want the length of the tag to be? 1 Byte or longer?
     * What should we do about payment channel vouchers? They are designed to live offchain for an extended period of time. We may have to invalidate older vouchers, which we may not want to do.
     * Can we make this with BLS aggregation?
### Network v17 Upgrade
#### Confirmed Fips
* [FIP0029 - Beneficiary Address for Storage Providers](https://github.com/filecoin-project/FIPs/blob/master/FIPS/fip-0029.md)
* [FIP0034 - Fix PreCommit Deposit Independent of Sector Content](https://github.com/filecoin-project/FIPs/blob/master/FIPS/fip-0034.md)
* [FIP0041 - Forward Compatibility for Pre-Commit](https://github.com/filecoin-project/FIPs/blob/master/FIPS/fip-0041.md)
* FIPXXXX - Decoupling Fil+ from Marketplace
* FIPXXXX - Signature Domain Separation
* FIPXXXX - New API for Builtin Actors Accessible to User-Programmed Actors
  *  (Alex N) proposed to defer or move to potential for this FIP. This FIP depends on negotiations with the FVM team over timeline.
  *  (Raul) commented that this will be a large endeavor. If we are not ready, and we ship FVM programmability and devs cannot program things they want to program, it will not go well. We need to have a user driven discussion and understand what the use cases are and make sure that APIs that we ship first cover 80% of use cases that devs are ready to build. FF may be able to help steward this conversation. Kaitlin recommended we start this conversation publicly.
#### Potential FIPS
* [FIP0036 - Introducing a Sector Duration Multiple for Longer Term Storage Commitment](https://github.com/filecoin-project/FIPs/blob/master/FIPS/fip-0036.md)
  * (Alex N) This invalidates the previous security policy on rotating sectors every 1 1/2 years. We need to have a security discussion prior to FIP0036 going into confirmed.
#### Timeline
* Butterflynet upgrade: End of Aug
* Calibnet upgrade: Mid-Sept
* Mainnet Upgrade: End of Sept
  * (Jennijuju) This timeline is too short as it means the code freeze would be the third week of August. Raul commented that his team may need until the end of October.
    
#### Next Steps:
* (Kaitlin) Clarify confirmed FIPS list
  * This may help confirm a harder timeline. Check the NV17 thread.
* (Kaitlin) Provide update for new and ongoing Governance items

**Q&A:**

**Appendix:**
* Ecosystem Technical Updates
* Lotus Roadmap & Enhancements
* Venus Roadmap & Enhancements
* Forest Roadmap & Enhancements
* Network Operations & Health Statistics

