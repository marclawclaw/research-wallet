# Changelog
# 0.0.1 build 52
- [#709] Better error handling in tests (#713)

# 0.0.1 build 51
- [#711] Transaction History not shown (#715) 

# 0.0.1 build 50
- [#707] Adopt latest SDK (#708)
- [#705] Transaction detail lacks memo and addresses (#706)
- [#265] Integrate App Rating Alert (#703)
- [#698] RootView to use SwitchStore (#699)
- [#691] Adopt sync/async synchronizer changes (#696)
- [#683] Zip log files into one (#692)
- [#684] Improvements for the derivation tool dependency (#689)
- [#678] Adopt TCA 0.52.0 (#688)
- [#682] Adopt removal of the Notification center on the SDK side (#687)

# 0.0.1 build 49
- [#673] End to end bugfix (#679)
Bugs fixed:
 - derivation tool live key has hardcoded mainnet so it doesn't recognise and validate zcash testnet addresses
 - send to transparent address fails because of Memo("") provided instead of nil
 - when transparent address is filled in a send form, the memo input is still present in the UI, memo is not supported by transparent addresses so it should be removed

# 0.0.1 build 48
- [#676] fix About.swift not being present on mainnet target (#677)
- [#654] Convert SDKSynchronizerDependency to regular TCA dependency (#672)
# 0.0.1 build 47
- [#653] Adopt SDK initialisation changes (#671)
- [#668] Balance Breakdown design enhancements (#669)
- [#660] Fix missing percentage on homepage while syncing (#670)
- [#663] Shield Funds button is enabled when there are no funds to shield (#665)
- [#666] Remove Graphics from "create new wallet" screen (#667)
- [#661] Send Button works even if it's apparently disabled (#664)
- [#660] Settings button is not part of a navigation bar (#662)
- [#658] About Screen with version (#659)
- [#652] Each logged TCA actions appears twice in the log (#657)

# 0.0.1 build 46
- [#626] Small UI-UX fixes for 0.0.1-45 (#649)
- [#650] Layout changes for the send screen (#651)
- [#647] Adopt 0.19.1-beta (#648)
- [#597] Sync cannot be retried after a failure (#646)
- [#631] Make Send Form fields avoid being blocked by keyboard (#645)
- [#599] Add ability to shield funds (#641)
- [#632] Show error message for failed transaction (#642)
- [#628] TAZ vs ZEC builds (#637)
- [#639] Show valid balance after app start (#640)
- [#618] Require specific version of SwiftGen (#638)
# 0.0.1 build 45
- [#635] Fix HomeTests
- [#633] build and release from tag 0.0.1-45
- [#611] Disable Send ZEC button when sync in progress 
- [#617] Use L10n for all the texts in the app (#627)
- [#594] Don't Allow user to proceed to send funds if they are not available for spend (#629)
- [#595] Visbility of fiat conversion on homeage depends on feature flag (#625)
- [#592] Add export logs to debug menu (#621)
- PR Fix how sync progress is displayed (#624)
- [#618] Use SwiftGen to generate L10n structure (#619)
