## Distro

Windows Server 2019


## GSSAPI Impl

SSPI


## Connection Info:

Connecting to SMB share using the FQDN. Uses explicit auth.


## Notes

* SMB auth is slightly different, it sends a `NegTokenInit2` structure from the server to client that lists all the mechs it supports
* The `NegTokenInit2` structure is slightly different than `NegTokenInit` as it includes the `negHints` field at the same tag `[3]` as `mechListMIC` in `NegTokenInit`
* The client seems to ignore the first message and just starts a branch new SPNEGO exchange with `NegTokenInit` in it's first reply
* No `mechListMIC` unlike NTLM.


## Tokens

```yaml
MessageType: SPNEGO InitialContextToken
Data:
  thisMech: SPNEGO (1.3.6.1.5.5.2)
  innerContextToken:
    MessageType: SPNEGO NegTokenInit2
    Data:
      mechTypes:
      - NEGOEX (1.3.6.1.4.1.311.2.2.30)
      - MS Kerberos (1.2.840.48018.1.2.2)
      - Kerberos (1.2.840.113554.1.2.2)
      - Kerberos User to User (1.2.840.113554.1.2.2.3)
      - NTLM (1.3.6.1.4.1.311.2.2.10)
      reqFlags:
      mechToken:
      mechListMIC:
      negHints:
        hintName: not_defined_in_RFC4178@please_ignore
        hintAddress:
    RawData: A06C306AA03C303A060A2B06010401823702021E06092A864882F71201020206092A864886F712010202060A2A864886F71201020203060A2B06010401823702020AA32A3028A0261B246E6F745F646566696E65645F696E5F5246433431373840706C656173655F69676E6F7265
RawData: 607606062B0601050502A06C306AA03C303A060A2B06010401823702021E06092A864882F71201020206092A864886F712010202060A2A864886F71201020203060A2B06010401823702020AA32A3028A0261B246E6F745F646566696E65645F696E5F5246433431373840706C656173655F69676E6F7265
```

```yaml
MessageType: SPNEGO InitialContextToken
Data:
  thisMech: SPNEGO (1.3.6.1.5.5.2)
  innerContextToken:
    MessageType: SPNEGO NegTokenInit
    Data:
      mechTypes:
      - MS Kerberos (1.2.840.48018.1.2.2)
      - Kerberos (1.2.840.113554.1.2.2)
      - NEGOEX (1.3.6.1.4.1.311.2.2.30)
      - NTLM (1.3.6.1.4.1.311.2.2.10)
      reqFlags:
      mechToken:
        MessageType: SPNEGO InitialContextToken
        Data:
          thisMech: Kerberos (1.2.840.113554.1.2.2)
          innerContextToken:
            MessageType: AP-REQ (14)
            Data:
              pvno: 5
              msg-type: AP-REQ (14)
              ap-options:
                raw: 32
                flags:
                - mutual-required (32)
              ticket:
                tkt-vno: 5
                realm: DOMAIN.LOCAL
                sname:
                  name-type: NT-SRV-INST (2)
                  name-string:
                  - cifs
                  - dc01.domain.local
                enc-part:
                  etype: AES256_CTS_HMAC_SHA1_96 (18)
                  kvno: 21
                  cipher: 50F329F883A5C120C321D8F8801A44954D6ABC7BE9BABC07B4EA7083739EC57D4FAE5F373662F69DECC0B0E12DBDF9E0FA8934AFDB34F5843C2E962F40AA93C99CB1FF88950B9EA5826C856C83FA17271AEBCDC8A0E3CA34C40E3D224B78664E6C5CFDAFF12E6907BB65B05C78E3C5AE51B4AC80566A33024989A4E77751E86B684D497596BFB410F37EF7B0A530F0F1BFFC1A49FA283B1D5DCF700805ADEC68A68AF0F1512679D757D3326C9B0DED9582ACF2BBF111774AF5E00952B5D73A88AA7FF2656C985EC79EA91B75DBF5DDAD28AC9813505D25DF782AD30A78038CBF148FB159FCB8C55B8DF6D49726F18ACB09E6221AE0D32838753B79A04B82F5758145243CDBAAD5667D3C9F4D6E62C97AFE576F1F2067C296DE23B8ADAA538B006D021463F20C3F2CB0D7D901F78456BCD8E15636E5295222110C8C18BD25C44FCB246E0804D794D1156BB50B848E83813B943E41A49CFA339B2B7D31DFDC2BFC6CFC01212BA895823C0ACE411AE061D69A40351A65DB9FDF68A586D42503BB5133E7919A20FA10128B346BAED3F5A76272DF5E1A9E9BC040C816B9F279BA357D4AA447E49E640AD17954BAE45F98B0A82E1931327F2F7A8E7F72CD0C5A4D81FAAB1DDF27A510D4CF44830ADE28504F1ECB1F8C083B3F05E31829F41599CA66826BFE6E327BC891305AF5BDBE7F1CE3BDA08150B22E708D896EC6BAA8219DD45F4F857B27AA9E9A10F94C059E66A157F7F511849C1B437DC519A9496266597EF86FD099514795BF5C59E44DB42D711AFE40DCCCFF37E24DC7273522B59793FC829A0E9ADA5FCFA36F85213BEAB3D5C3EA5448AAF20213C5F94DDA9DE6DED09A4F5899C33852E6F8331B222215B818A73A97CF799189AAA0BF9859A7F20435BD43810ADC25F0275BC1040D595E3B86B647CE47DACD8D685F36413C09C29C7192C9B678E45A5FB32A6A31F4B4557932D268E343E4D672E9832318E40B487C7C8636001C860C24ED9A754A07EC65304B488F32AE77CB4243C9EC130B2C9B475D9D7F33E4FABEF4AF9910BE76552BB940AC0793585C234C9A5E99AB640A3EE729729B52891DAD2C2A12306181B9BB066914B73AB67EDECC8B7C3BD5EA45CC0E7D4A205ECA4A150A3FE69D466203AED2462898365C522ECD0FDB73EDF97A6EFCD757AE8E240AF3618DC019D686171709712F3319C86DF91A42C9FFE3CDD1B8FA6568329410258045CF703B2A3F20636A0A06C4CE05D0D53A57A967F49C0FE7C94006658ADD6899FF9AD5AD40823146DE64EEE6E36C4DCD8A27418991733FE64E7FB87957566A5533A4DE67B06C5B52947E076FEC660B07EA68BF282686C9B6B45ED4442B815CC1CA76743985A509ADF5BB79F6B4BCA5F460C6F6A8822EC7ADD5BF7B13518D5CC561F52DCAC0655D9F6942809625471FC5B8B22E9A257F947B7012C4B14917DE08C3366B138B1D23D4CD8D5DBE2A9A6399CDFA9AA0D46ADE177FC6E810A7E181DA89F11883CCC1D5C8ACE12DD47F44FA17EAAFF49C2907AE6D0CDD97161B718293CF687A2018093A4349B343880318ABB9176F2D5347C970A70C6560128B9D37C8C326899B5801BD402F1CEB55AFE2742C2F5BCEF48F
              authenticator:
                etype: AES256_CTS_HMAC_SHA1_96 (18)
                kvno:
                cipher: 14F115ACDAD14B8C5A344412BD99B7FAC1786CCA4C06936FF986F108E0872E4B723C517175A46FD7C2D0F2C985424BEABF3B07BE1AC4A9EF49AA0C58F7F028EA99D52E7E65AE544CB68E57FFBD0ECE96D3420D56683CF6B4083BBA98B9B632C1C49A8EA8EE3F52D5086C470CCE57F5D0954987CE91F02A8239D7C64BD0D2A401BEDCA94C520A3B9BD5907BE6578A50865F2F933F4B5DFB7D547499119D2C3F21BC3D86836F7F585440D8596A76CB3B8D9EA5236CB56FF1319D727FD0406D7F3B868CAD5976A596097305AEA3055A61E5C76B2C155137F195F0CF190413FB8E07DFC28F287647EA1EF13FF834630F84C7011EFDD45EB899A5410D7CBD1665192862E53C2DA7712976750763A8B86DC48740F52D015E7F0874EA0D039D567D5E4A7BCFFA7FE8771A06B5ACF2FB67D347924C4CEE5D6CCF785D5FAF0116BACECAB61C12CD554AD21984ACE71C568DF67669B33E03D6771924759E43F0C14A0C4EC558CD91FA903DC9E93D0B6926DD11E40EC526E743C8AEC021DE04CD664CC247BE93022C53AD32DB9360EAB7D9A1013E41F736807855AB9A43FC4A6389CDC936B8939D1B915A515D881911F97DE4337BFE371BB0D7FA41C9F6B196412080CDD0D391F026BDB9D620D1AD475820D1B1F89C11CF739694F9DCD7C3B44EB1ECB46EBF6ACC17D4461BB6C1E3291C3DE8A311C4BFA4059FFAAEEBB645605E9DA3ED6F97BED7723BCD3BC87975886F792EAA8CFAB23886EF84F3D52B2E21E1F01B334F80B55F5ECFA0AE08DAA2A04C819050445574608E54B863E448560D236BA66C55F6044E91704DFDC8C21A06C42952A30D8144B91F2E3654722589571DF866244B7B8E7AA6157AD1D2B5D8C432C790001511788E16077B203F8E24594834F5A42BB365EEF2CA9F8EA5316D1A43D5AD10F1B0F59635B1C21B7850B72BE0132A3BAC7A45A862B0CC9A560430B03DF66A81743A2E3F2D19366A3AA460C6047EC7C64C56AE0C1747601B5F2E3CAD15C400F1260F3272DEE08545B97A49B2FF34C89BD1D8D206F5B68A91546ABB86C4162080434DA01E64C635BF293F2053B3CB4F5B1506186C44B4462CE1279D07D39E44529479B675BC77BBB29CC34D20A0FD9924E6331C27A413DF133E47BFF2BA28F54BF6732CBF70762158172926B44312F67023842DD907E1EFE3F31D739FAD89B46EFC1E0C22E623E21A79A1732CEEF9658D602BAB4FC11153CDAD108C65F626FBC5DEBE3F8A2F390F0CC96C1002AF229E4251F4BAB199D641E489521E6D1741DA73FDB660C295CDF6E076B5D8A95C82909F8D79A429E75B7045785DE3527786F82FAF66DFD0E97C39B205305545F2A9B6F1A75B639BE3ABCB470C4BD30E0BE589B71BE7F5C61FE4CF92B8788B81ADAEDF95DB6EE6A7E7E8C6E891C9EBB5FDD0D27868F2127FCBA230E30F8DF051BF0CF4BDC454F58DF5FF8F88FC4B39A296ABF3DBDF3F5009802677DE5822876F345AFD7E9C860F3F076ACD95291781BB9945754A90C0915812A804FB16EE8130DB45494FC13310618CB509B6C097FBFD1160C4847618E459A327E2248185C96DBEFE4CBAE0B7B3509C3CD65E699C649022ADCD90CF39BCA993D61EC3FE153A2EC0A6D5CA6122DC691438B2A3BC9DADA33DE5457F581ADAB588EAE53147F42AE752CAB54BDC118ED04F5A6ED388A0CE737EEEF115CD53885194C5F701D97742A24998EF6A9F2EF2CAA291AC1E6A13F8EC262E11DFD4A0382EE918BB4D45C9E1C89C843976F4449688FEBA14068A1AEFEE924F1286E124EC574B2B0CAD580E1B80CEA3A45935D101BDC6CB6367088D0C233064025226747C9B6D5364121D968ACDC0852E2AAD1B13E7F69265E82FC017CE9A270F5AABB40C83E8F88BB12F542522905AD4854C636C24C02A3D5CEE179803C436FE7EF65003201D528440FC51975220A1AEF8E2576D1E903D79DBF744BCDDDBEA84E69745C46E669B018880A2DC80ED23022DCAA02D37E7322CB596DB9FE8B7C4046547459FA365B803A84F04A7F8011B0DDFEFD93F2D327FD52AAD47E95118D2275AAF1668E8C87B3FC805D7BB29DA839D4EE4C88AFF43DC0EBD99F1E6AF7DAEEE3839405D736CDA68338B70893D67DFE7CAB5A29212700DF7CF0BA335C93B756E1553E0B8125D7D60FAB67E3FB2FC4F87684BE2AB0D1DCAAA6834EDAB22E45F9AD177A207F0AF34D4A462273A32853CBDEDA13E4618ED01FC126A748757224FC5BAD9865899B6E33930B6CC13023222035084367A8B3477EEDCF4B0917BBDF8EC8CF3A3F9E671E1741C530C9AF456ADB139A2C55B4E5FBF77E211EBA09A38545A21B0D2287104DFCC1C791F6E7F0C53EC230647002F705516E6479000294819710BB89ABB5305B8BAD87085022F004A77C43F996D75F15950C66F6A29F187A99805783E7A60CAF5C6212FAE7A86E4CC7A37C917B97B6330704C7E858E5E12FE9E48500632B51B2DDD2A29A526ABFD4D64F7C5CF3E5F80B4FFEBCFDB73394A59AF086BD670A7573CBA3D8B6EDA9B5441FC918C77D91337A7CF21F953624023D3728FB05297D1562B18
            RawData: 01006E820C1B30820C17A003020105A10302010EA20703050020000000A38204D6618204D2308204CEA003020105A10E1B0C444F4D41494E2E4C4F43414CA2243022A003020102A11B30191B04636966731B11646330312E646F6D61696E2E6C6F63616CA382048F3082048BA003020112A103020115A282047D0482047950F329F883A5C120C321D8F8801A44954D6ABC7BE9BABC07B4EA7083739EC57D4FAE5F373662F69DECC0B0E12DBDF9E0FA8934AFDB34F5843C2E962F40AA93C99CB1FF88950B9EA5826C856C83FA17271AEBCDC8A0E3CA34C40E3D224B78664E6C5CFDAFF12E6907BB65B05C78E3C5AE51B4AC80566A33024989A4E77751E86B684D497596BFB410F37EF7B0A530F0F1BFFC1A49FA283B1D5DCF700805ADEC68A68AF0F1512679D757D3326C9B0DED9582ACF2BBF111774AF5E00952B5D73A88AA7FF2656C985EC79EA91B75DBF5DDAD28AC9813505D25DF782AD30A78038CBF148FB159FCB8C55B8DF6D49726F18ACB09E6221AE0D32838753B79A04B82F5758145243CDBAAD5667D3C9F4D6E62C97AFE576F1F2067C296DE23B8ADAA538B006D021463F20C3F2CB0D7D901F78456BCD8E15636E5295222110C8C18BD25C44FCB246E0804D794D1156BB50B848E83813B943E41A49CFA339B2B7D31DFDC2BFC6CFC01212BA895823C0ACE411AE061D69A40351A65DB9FDF68A586D42503BB5133E7919A20FA10128B346BAED3F5A76272DF5E1A9E9BC040C816B9F279BA357D4AA447E49E640AD17954BAE45F98B0A82E1931327F2F7A8E7F72CD0C5A4D81FAAB1DDF27A510D4CF44830ADE28504F1ECB1F8C083B3F05E31829F41599CA66826BFE6E327BC891305AF5BDBE7F1CE3BDA08150B22E708D896EC6BAA8219DD45F4F857B27AA9E9A10F94C059E66A157F7F511849C1B437DC519A9496266597EF86FD099514795BF5C59E44DB42D711AFE40DCCCFF37E24DC7273522B59793FC829A0E9ADA5FCFA36F85213BEAB3D5C3EA5448AAF20213C5F94DDA9DE6DED09A4F5899C33852E6F8331B222215B818A73A97CF799189AAA0BF9859A7F20435BD43810ADC25F0275BC1040D595E3B86B647CE47DACD8D685F36413C09C29C7192C9B678E45A5FB32A6A31F4B4557932D268E343E4D672E9832318E40B487C7C8636001C860C24ED9A754A07EC65304B488F32AE77CB4243C9EC130B2C9B475D9D7F33E4FABEF4AF9910BE76552BB940AC0793585C234C9A5E99AB640A3EE729729B52891DAD2C2A12306181B9BB066914B73AB67EDECC8B7C3BD5EA45CC0E7D4A205ECA4A150A3FE69D466203AED2462898365C522ECD0FDB73EDF97A6EFCD757AE8E240AF3618DC019D686171709712F3319C86DF91A42C9FFE3CDD1B8FA6568329410258045CF703B2A3F20636A0A06C4CE05D0D53A57A967F49C0FE7C94006658ADD6899FF9AD5AD40823146DE64EEE6E36C4DCD8A27418991733FE64E7FB87957566A5533A4DE67B06C5B52947E076FEC660B07EA68BF282686C9B6B45ED4442B815CC1CA76743985A509ADF5BB79F6B4BCA5F460C6F6A8822EC7ADD5BF7B13518D5CC561F52DCAC0655D9F6942809625471FC5B8B22E9A257F947B7012C4B14917DE08C3366B138B1D23D4CD8D5DBE2A9A6399CDFA9AA0D46ADE177FC6E810A7E181DA89F11883CCC1D5C8ACE12DD47F44FA17EAAFF49C2907AE6D0CDD97161B718293CF687A2018093A4349B343880318ABB9176F2D5347C970A70C6560128B9D37C8C326899B5801BD402F1CEB55AFE2742C2F5BCEF48FA482072630820722A003020112A28207190482071514F115ACDAD14B8C5A344412BD99B7FAC1786CCA4C06936FF986F108E0872E4B723C517175A46FD7C2D0F2C985424BEABF3B07BE1AC4A9EF49AA0C58F7F028EA99D52E7E65AE544CB68E57FFBD0ECE96D3420D56683CF6B4083BBA98B9B632C1C49A8EA8EE3F52D5086C470CCE57F5D0954987CE91F02A8239D7C64BD0D2A401BEDCA94C520A3B9BD5907BE6578A50865F2F933F4B5DFB7D547499119D2C3F21BC3D86836F7F585440D8596A76CB3B8D9EA5236CB56FF1319D727FD0406D7F3B868CAD5976A596097305AEA3055A61E5C76B2C155137F195F0CF190413FB8E07DFC28F287647EA1EF13FF834630F84C7011EFDD45EB899A5410D7CBD1665192862E53C2DA7712976750763A8B86DC48740F52D015E7F0874EA0D039D567D5E4A7BCFFA7FE8771A06B5ACF2FB67D347924C4CEE5D6CCF785D5FAF0116BACECAB61C12CD554AD21984ACE71C568DF67669B33E03D6771924759E43F0C14A0C4EC558CD91FA903DC9E93D0B6926DD11E40EC526E743C8AEC021DE04CD664CC247BE93022C53AD32DB9360EAB7D9A1013E41F736807855AB9A43FC4A6389CDC936B8939D1B915A515D881911F97DE4337BFE371BB0D7FA41C9F6B196412080CDD0D391F026BDB9D620D1AD475820D1B1F89C11CF739694F9DCD7C3B44EB1ECB46EBF6ACC17D4461BB6C1E3291C3DE8A311C4BFA4059FFAAEEBB645605E9DA3ED6F97BED7723BCD3BC87975886F792EAA8CFAB23886EF84F3D52B2E21E1F01B334F80B55F5ECFA0AE08DAA2A04C819050445574608E54B863E448560D236BA66C55F6044E91704DFDC8C21A06C42952A30D8144B91F2E3654722589571DF866244B7B8E7AA6157AD1D2B5D8C432C790001511788E16077B203F8E24594834F5A42BB365EEF2CA9F8EA5316D1A43D5AD10F1B0F59635B1C21B7850B72BE0132A3BAC7A45A862B0CC9A560430B03DF66A81743A2E3F2D19366A3AA460C6047EC7C64C56AE0C1747601B5F2E3CAD15C400F1260F3272DEE08545B97A49B2FF34C89BD1D8D206F5B68A91546ABB86C4162080434DA01E64C635BF293F2053B3CB4F5B1506186C44B4462CE1279D07D39E44529479B675BC77BBB29CC34D20A0FD9924E6331C27A413DF133E47BFF2BA28F54BF6732CBF70762158172926B44312F67023842DD907E1EFE3F31D739FAD89B46EFC1E0C22E623E21A79A1732CEEF9658D602BAB4FC11153CDAD108C65F626FBC5DEBE3F8A2F390F0CC96C1002AF229E4251F4BAB199D641E489521E6D1741DA73FDB660C295CDF6E076B5D8A95C82909F8D79A429E75B7045785DE3527786F82FAF66DFD0E97C39B205305545F2A9B6F1A75B639BE3ABCB470C4BD30E0BE589B71BE7F5C61FE4CF92B8788B81ADAEDF95DB6EE6A7E7E8C6E891C9EBB5FDD0D27868F2127FCBA230E30F8DF051BF0CF4BDC454F58DF5FF8F88FC4B39A296ABF3DBDF3F5009802677DE5822876F345AFD7E9C860F3F076ACD95291781BB9945754A90C0915812A804FB16EE8130DB45494FC13310618CB509B6C097FBFD1160C4847618E459A327E2248185C96DBEFE4CBAE0B7B3509C3CD65E699C649022ADCD90CF39BCA993D61EC3FE153A2EC0A6D5CA6122DC691438B2A3BC9DADA33DE5457F581ADAB588EAE53147F42AE752CAB54BDC118ED04F5A6ED388A0CE737EEEF115CD53885194C5F701D97742A24998EF6A9F2EF2CAA291AC1E6A13F8EC262E11DFD4A0382EE918BB4D45C9E1C89C843976F4449688FEBA14068A1AEFEE924F1286E124EC574B2B0CAD580E1B80CEA3A45935D101BDC6CB6367088D0C233064025226747C9B6D5364121D968ACDC0852E2AAD1B13E7F69265E82FC017CE9A270F5AABB40C83E8F88BB12F542522905AD4854C636C24C02A3D5CEE179803C436FE7EF65003201D528440FC51975220A1AEF8E2576D1E903D79DBF744BCDDDBEA84E69745C46E669B018880A2DC80ED23022DCAA02D37E7322CB596DB9FE8B7C4046547459FA365B803A84F04A7F8011B0DDFEFD93F2D327FD52AAD47E95118D2275AAF1668E8C87B3FC805D7BB29DA839D4EE4C88AFF43DC0EBD99F1E6AF7DAEEE3839405D736CDA68338B70893D67DFE7CAB5A29212700DF7CF0BA335C93B756E1553E0B8125D7D60FAB67E3FB2FC4F87684BE2AB0D1DCAAA6834EDAB22E45F9AD177A207F0AF34D4A462273A32853CBDEDA13E4618ED01FC126A748757224FC5BAD9865899B6E33930B6CC13023222035084367A8B3477EEDCF4B0917BBDF8EC8CF3A3F9E671E1741C530C9AF456ADB139A2C55B4E5FBF77E211EBA09A38545A21B0D2287104DFCC1C791F6E7F0C53EC230647002F705516E6479000294819710BB89ABB5305B8BAD87085022F004A77C43F996D75F15950C66F6A29F187A99805783E7A60CAF5C6212FAE7A86E4CC7A37C917B97B6330704C7E858E5E12FE9E48500632B51B2DDD2A29A526ABFD4D64F7C5CF3E5F80B4FFEBCFDB73394A59AF086BD670A7573CBA3D8B6EDA9B5441FC918C77D91337A7CF21F953624023D3728FB05297D1562B18
        RawData: 60820C2C06092A864886F71201020201006E820C1B30820C17A003020105A10302010EA20703050020000000A38204D6618204D2308204CEA003020105A10E1B0C444F4D41494E2E4C4F43414CA2243022A003020102A11B30191B04636966731B11646330312E646F6D61696E2E6C6F63616CA382048F3082048BA003020112A103020115A282047D0482047950F329F883A5C120C321D8F8801A44954D6ABC7BE9BABC07B4EA7083739EC57D4FAE5F373662F69DECC0B0E12DBDF9E0FA8934AFDB34F5843C2E962F40AA93C99CB1FF88950B9EA5826C856C83FA17271AEBCDC8A0E3CA34C40E3D224B78664E6C5CFDAFF12E6907BB65B05C78E3C5AE51B4AC80566A33024989A4E77751E86B684D497596BFB410F37EF7B0A530F0F1BFFC1A49FA283B1D5DCF700805ADEC68A68AF0F1512679D757D3326C9B0DED9582ACF2BBF111774AF5E00952B5D73A88AA7FF2656C985EC79EA91B75DBF5DDAD28AC9813505D25DF782AD30A78038CBF148FB159FCB8C55B8DF6D49726F18ACB09E6221AE0D32838753B79A04B82F5758145243CDBAAD5667D3C9F4D6E62C97AFE576F1F2067C296DE23B8ADAA538B006D021463F20C3F2CB0D7D901F78456BCD8E15636E5295222110C8C18BD25C44FCB246E0804D794D1156BB50B848E83813B943E41A49CFA339B2B7D31DFDC2BFC6CFC01212BA895823C0ACE411AE061D69A40351A65DB9FDF68A586D42503BB5133E7919A20FA10128B346BAED3F5A76272DF5E1A9E9BC040C816B9F279BA357D4AA447E49E640AD17954BAE45F98B0A82E1931327F2F7A8E7F72CD0C5A4D81FAAB1DDF27A510D4CF44830ADE28504F1ECB1F8C083B3F05E31829F41599CA66826BFE6E327BC891305AF5BDBE7F1CE3BDA08150B22E708D896EC6BAA8219DD45F4F857B27AA9E9A10F94C059E66A157F7F511849C1B437DC519A9496266597EF86FD099514795BF5C59E44DB42D711AFE40DCCCFF37E24DC7273522B59793FC829A0E9ADA5FCFA36F85213BEAB3D5C3EA5448AAF20213C5F94DDA9DE6DED09A4F5899C33852E6F8331B222215B818A73A97CF799189AAA0BF9859A7F20435BD43810ADC25F0275BC1040D595E3B86B647CE47DACD8D685F36413C09C29C7192C9B678E45A5FB32A6A31F4B4557932D268E343E4D672E9832318E40B487C7C8636001C860C24ED9A754A07EC65304B488F32AE77CB4243C9EC130B2C9B475D9D7F33E4FABEF4AF9910BE76552BB940AC0793585C234C9A5E99AB640A3EE729729B52891DAD2C2A12306181B9BB066914B73AB67EDECC8B7C3BD5EA45CC0E7D4A205ECA4A150A3FE69D466203AED2462898365C522ECD0FDB73EDF97A6EFCD757AE8E240AF3618DC019D686171709712F3319C86DF91A42C9FFE3CDD1B8FA6568329410258045CF703B2A3F20636A0A06C4CE05D0D53A57A967F49C0FE7C94006658ADD6899FF9AD5AD40823146DE64EEE6E36C4DCD8A27418991733FE64E7FB87957566A5533A4DE67B06C5B52947E076FEC660B07EA68BF282686C9B6B45ED4442B815CC1CA76743985A509ADF5BB79F6B4BCA5F460C6F6A8822EC7ADD5BF7B13518D5CC561F52DCAC0655D9F6942809625471FC5B8B22E9A257F947B7012C4B14917DE08C3366B138B1D23D4CD8D5DBE2A9A6399CDFA9AA0D46ADE177FC6E810A7E181DA89F11883CCC1D5C8ACE12DD47F44FA17EAAFF49C2907AE6D0CDD97161B718293CF687A2018093A4349B343880318ABB9176F2D5347C970A70C6560128B9D37C8C326899B5801BD402F1CEB55AFE2742C2F5BCEF48FA482072630820722A003020112A28207190482071514F115ACDAD14B8C5A344412BD99B7FAC1786CCA4C06936FF986F108E0872E4B723C517175A46FD7C2D0F2C985424BEABF3B07BE1AC4A9EF49AA0C58F7F028EA99D52E7E65AE544CB68E57FFBD0ECE96D3420D56683CF6B4083BBA98B9B632C1C49A8EA8EE3F52D5086C470CCE57F5D0954987CE91F02A8239D7C64BD0D2A401BEDCA94C520A3B9BD5907BE6578A50865F2F933F4B5DFB7D547499119D2C3F21BC3D86836F7F585440D8596A76CB3B8D9EA5236CB56FF1319D727FD0406D7F3B868CAD5976A596097305AEA3055A61E5C76B2C155137F195F0CF190413FB8E07DFC28F287647EA1EF13FF834630F84C7011EFDD45EB899A5410D7CBD1665192862E53C2DA7712976750763A8B86DC48740F52D015E7F0874EA0D039D567D5E4A7BCFFA7FE8771A06B5ACF2FB67D347924C4CEE5D6CCF785D5FAF0116BACECAB61C12CD554AD21984ACE71C568DF67669B33E03D6771924759E43F0C14A0C4EC558CD91FA903DC9E93D0B6926DD11E40EC526E743C8AEC021DE04CD664CC247BE93022C53AD32DB9360EAB7D9A1013E41F736807855AB9A43FC4A6389CDC936B8939D1B915A515D881911F97DE4337BFE371BB0D7FA41C9F6B196412080CDD0D391F026BDB9D620D1AD475820D1B1F89C11CF739694F9DCD7C3B44EB1ECB46EBF6ACC17D4461BB6C1E3291C3DE8A311C4BFA4059FFAAEEBB645605E9DA3ED6F97BED7723BCD3BC87975886F792EAA8CFAB23886EF84F3D52B2E21E1F01B334F80B55F5ECFA0AE08DAA2A04C819050445574608E54B863E448560D236BA66C55F6044E91704DFDC8C21A06C42952A30D8144B91F2E3654722589571DF866244B7B8E7AA6157AD1D2B5D8C432C790001511788E16077B203F8E24594834F5A42BB365EEF2CA9F8EA5316D1A43D5AD10F1B0F59635B1C21B7850B72BE0132A3BAC7A45A862B0CC9A560430B03DF66A81743A2E3F2D19366A3AA460C6047EC7C64C56AE0C1747601B5F2E3CAD15C400F1260F3272DEE08545B97A49B2FF34C89BD1D8D206F5B68A91546ABB86C4162080434DA01E64C635BF293F2053B3CB4F5B1506186C44B4462CE1279D07D39E44529479B675BC77BBB29CC34D20A0FD9924E6331C27A413DF133E47BFF2BA28F54BF6732CBF70762158172926B44312F67023842DD907E1EFE3F31D739FAD89B46EFC1E0C22E623E21A79A1732CEEF9658D602BAB4FC11153CDAD108C65F626FBC5DEBE3F8A2F390F0CC96C1002AF229E4251F4BAB199D641E489521E6D1741DA73FDB660C295CDF6E076B5D8A95C82909F8D79A429E75B7045785DE3527786F82FAF66DFD0E97C39B205305545F2A9B6F1A75B639BE3ABCB470C4BD30E0BE589B71BE7F5C61FE4CF92B8788B81ADAEDF95DB6EE6A7E7E8C6E891C9EBB5FDD0D27868F2127FCBA230E30F8DF051BF0CF4BDC454F58DF5FF8F88FC4B39A296ABF3DBDF3F5009802677DE5822876F345AFD7E9C860F3F076ACD95291781BB9945754A90C0915812A804FB16EE8130DB45494FC13310618CB509B6C097FBFD1160C4847618E459A327E2248185C96DBEFE4CBAE0B7B3509C3CD65E699C649022ADCD90CF39BCA993D61EC3FE153A2EC0A6D5CA6122DC691438B2A3BC9DADA33DE5457F581ADAB588EAE53147F42AE752CAB54BDC118ED04F5A6ED388A0CE737EEEF115CD53885194C5F701D97742A24998EF6A9F2EF2CAA291AC1E6A13F8EC262E11DFD4A0382EE918BB4D45C9E1C89C843976F4449688FEBA14068A1AEFEE924F1286E124EC574B2B0CAD580E1B80CEA3A45935D101BDC6CB6367088D0C233064025226747C9B6D5364121D968ACDC0852E2AAD1B13E7F69265E82FC017CE9A270F5AABB40C83E8F88BB12F542522905AD4854C636C24C02A3D5CEE179803C436FE7EF65003201D528440FC51975220A1AEF8E2576D1E903D79DBF744BCDDDBEA84E69745C46E669B018880A2DC80ED23022DCAA02D37E7322CB596DB9FE8B7C4046547459FA365B803A84F04A7F8011B0DDFEFD93F2D327FD52AAD47E95118D2275AAF1668E8C87B3FC805D7BB29DA839D4EE4C88AFF43DC0EBD99F1E6AF7DAEEE3839405D736CDA68338B70893D67DFE7CAB5A29212700DF7CF0BA335C93B756E1553E0B8125D7D60FAB67E3FB2FC4F87684BE2AB0D1DCAAA6834EDAB22E45F9AD177A207F0AF34D4A462273A32853CBDEDA13E4618ED01FC126A748757224FC5BAD9865899B6E33930B6CC13023222035084367A8B3477EEDCF4B0917BBDF8EC8CF3A3F9E671E1741C530C9AF456ADB139A2C55B4E5FBF77E211EBA09A38545A21B0D2287104DFCC1C791F6E7F0C53EC230647002F705516E6479000294819710BB89ABB5305B8BAD87085022F004A77C43F996D75F15950C66F6A29F187A99805783E7A60CAF5C6212FAE7A86E4CC7A37C917B97B6330704C7E858E5E12FE9E48500632B51B2DDD2A29A526ABFD4D64F7C5CF3E5F80B4FFEBCFDB73394A59AF086BD670A7573CBA3D8B6EDA9B5441FC918C77D91337A7CF21F953624023D3728FB05297D1562B18
      mechListMIC:
    RawData: A0820C6E30820C6AA030302E06092A864882F71201020206092A864886F712010202060A2B06010401823702021E060A2B06010401823702020AA2820C3404820C3060820C2C06092A864886F71201020201006E820C1B30820C17A003020105A10302010EA20703050020000000A38204D6618204D2308204CEA003020105A10E1B0C444F4D41494E2E4C4F43414CA2243022A003020102A11B30191B04636966731B11646330312E646F6D61696E2E6C6F63616CA382048F3082048BA003020112A103020115A282047D0482047950F329F883A5C120C321D8F8801A44954D6ABC7BE9BABC07B4EA7083739EC57D4FAE5F373662F69DECC0B0E12DBDF9E0FA8934AFDB34F5843C2E962F40AA93C99CB1FF88950B9EA5826C856C83FA17271AEBCDC8A0E3CA34C40E3D224B78664E6C5CFDAFF12E6907BB65B05C78E3C5AE51B4AC80566A33024989A4E77751E86B684D497596BFB410F37EF7B0A530F0F1BFFC1A49FA283B1D5DCF700805ADEC68A68AF0F1512679D757D3326C9B0DED9582ACF2BBF111774AF5E00952B5D73A88AA7FF2656C985EC79EA91B75DBF5DDAD28AC9813505D25DF782AD30A78038CBF148FB159FCB8C55B8DF6D49726F18ACB09E6221AE0D32838753B79A04B82F5758145243CDBAAD5667D3C9F4D6E62C97AFE576F1F2067C296DE23B8ADAA538B006D021463F20C3F2CB0D7D901F78456BCD8E15636E5295222110C8C18BD25C44FCB246E0804D794D1156BB50B848E83813B943E41A49CFA339B2B7D31DFDC2BFC6CFC01212BA895823C0ACE411AE061D69A40351A65DB9FDF68A586D42503BB5133E7919A20FA10128B346BAED3F5A76272DF5E1A9E9BC040C816B9F279BA357D4AA447E49E640AD17954BAE45F98B0A82E1931327F2F7A8E7F72CD0C5A4D81FAAB1DDF27A510D4CF44830ADE28504F1ECB1F8C083B3F05E31829F41599CA66826BFE6E327BC891305AF5BDBE7F1CE3BDA08150B22E708D896EC6BAA8219DD45F4F857B27AA9E9A10F94C059E66A157F7F511849C1B437DC519A9496266597EF86FD099514795BF5C59E44DB42D711AFE40DCCCFF37E24DC7273522B59793FC829A0E9ADA5FCFA36F85213BEAB3D5C3EA5448AAF20213C5F94DDA9DE6DED09A4F5899C33852E6F8331B222215B818A73A97CF799189AAA0BF9859A7F20435BD43810ADC25F0275BC1040D595E3B86B647CE47DACD8D685F36413C09C29C7192C9B678E45A5FB32A6A31F4B4557932D268E343E4D672E9832318E40B487C7C8636001C860C24ED9A754A07EC65304B488F32AE77CB4243C9EC130B2C9B475D9D7F33E4FABEF4AF9910BE76552BB940AC0793585C234C9A5E99AB640A3EE729729B52891DAD2C2A12306181B9BB066914B73AB67EDECC8B7C3BD5EA45CC0E7D4A205ECA4A150A3FE69D466203AED2462898365C522ECD0FDB73EDF97A6EFCD757AE8E240AF3618DC019D686171709712F3319C86DF91A42C9FFE3CDD1B8FA6568329410258045CF703B2A3F20636A0A06C4CE05D0D53A57A967F49C0FE7C94006658ADD6899FF9AD5AD40823146DE64EEE6E36C4DCD8A27418991733FE64E7FB87957566A5533A4DE67B06C5B52947E076FEC660B07EA68BF282686C9B6B45ED4442B815CC1CA76743985A509ADF5BB79F6B4BCA5F460C6F6A8822EC7ADD5BF7B13518D5CC561F52DCAC0655D9F6942809625471FC5B8B22E9A257F947B7012C4B14917DE08C3366B138B1D23D4CD8D5DBE2A9A6399CDFA9AA0D46ADE177FC6E810A7E181DA89F11883CCC1D5C8ACE12DD47F44FA17EAAFF49C2907AE6D0CDD97161B718293CF687A2018093A4349B343880318ABB9176F2D5347C970A70C6560128B9D37C8C326899B5801BD402F1CEB55AFE2742C2F5BCEF48FA482072630820722A003020112A28207190482071514F115ACDAD14B8C5A344412BD99B7FAC1786CCA4C06936FF986F108E0872E4B723C517175A46FD7C2D0F2C985424BEABF3B07BE1AC4A9EF49AA0C58F7F028EA99D52E7E65AE544CB68E57FFBD0ECE96D3420D56683CF6B4083BBA98B9B632C1C49A8EA8EE3F52D5086C470CCE57F5D0954987CE91F02A8239D7C64BD0D2A401BEDCA94C520A3B9BD5907BE6578A50865F2F933F4B5DFB7D547499119D2C3F21BC3D86836F7F585440D8596A76CB3B8D9EA5236CB56FF1319D727FD0406D7F3B868CAD5976A596097305AEA3055A61E5C76B2C155137F195F0CF190413FB8E07DFC28F287647EA1EF13FF834630F84C7011EFDD45EB899A5410D7CBD1665192862E53C2DA7712976750763A8B86DC48740F52D015E7F0874EA0D039D567D5E4A7BCFFA7FE8771A06B5ACF2FB67D347924C4CEE5D6CCF785D5FAF0116BACECAB61C12CD554AD21984ACE71C568DF67669B33E03D6771924759E43F0C14A0C4EC558CD91FA903DC9E93D0B6926DD11E40EC526E743C8AEC021DE04CD664CC247BE93022C53AD32DB9360EAB7D9A1013E41F736807855AB9A43FC4A6389CDC936B8939D1B915A515D881911F97DE4337BFE371BB0D7FA41C9F6B196412080CDD0D391F026BDB9D620D1AD475820D1B1F89C11CF739694F9DCD7C3B44EB1ECB46EBF6ACC17D4461BB6C1E3291C3DE8A311C4BFA4059FFAAEEBB645605E9DA3ED6F97BED7723BCD3BC87975886F792EAA8CFAB23886EF84F3D52B2E21E1F01B334F80B55F5ECFA0AE08DAA2A04C819050445574608E54B863E448560D236BA66C55F6044E91704DFDC8C21A06C42952A30D8144B91F2E3654722589571DF866244B7B8E7AA6157AD1D2B5D8C432C790001511788E16077B203F8E24594834F5A42BB365EEF2CA9F8EA5316D1A43D5AD10F1B0F59635B1C21B7850B72BE0132A3BAC7A45A862B0CC9A560430B03DF66A81743A2E3F2D19366A3AA460C6047EC7C64C56AE0C1747601B5F2E3CAD15C400F1260F3272DEE08545B97A49B2FF34C89BD1D8D206F5B68A91546ABB86C4162080434DA01E64C635BF293F2053B3CB4F5B1506186C44B4462CE1279D07D39E44529479B675BC77BBB29CC34D20A0FD9924E6331C27A413DF133E47BFF2BA28F54BF6732CBF70762158172926B44312F67023842DD907E1EFE3F31D739FAD89B46EFC1E0C22E623E21A79A1732CEEF9658D602BAB4FC11153CDAD108C65F626FBC5DEBE3F8A2F390F0CC96C1002AF229E4251F4BAB199D641E489521E6D1741DA73FDB660C295CDF6E076B5D8A95C82909F8D79A429E75B7045785DE3527786F82FAF66DFD0E97C39B205305545F2A9B6F1A75B639BE3ABCB470C4BD30E0BE589B71BE7F5C61FE4CF92B8788B81ADAEDF95DB6EE6A7E7E8C6E891C9EBB5FDD0D27868F2127FCBA230E30F8DF051BF0CF4BDC454F58DF5FF8F88FC4B39A296ABF3DBDF3F5009802677DE5822876F345AFD7E9C860F3F076ACD95291781BB9945754A90C0915812A804FB16EE8130DB45494FC13310618CB509B6C097FBFD1160C4847618E459A327E2248185C96DBEFE4CBAE0B7B3509C3CD65E699C649022ADCD90CF39BCA993D61EC3FE153A2EC0A6D5CA6122DC691438B2A3BC9DADA33DE5457F581ADAB588EAE53147F42AE752CAB54BDC118ED04F5A6ED388A0CE737EEEF115CD53885194C5F701D97742A24998EF6A9F2EF2CAA291AC1E6A13F8EC262E11DFD4A0382EE918BB4D45C9E1C89C843976F4449688FEBA14068A1AEFEE924F1286E124EC574B2B0CAD580E1B80CEA3A45935D101BDC6CB6367088D0C233064025226747C9B6D5364121D968ACDC0852E2AAD1B13E7F69265E82FC017CE9A270F5AABB40C83E8F88BB12F542522905AD4854C636C24C02A3D5CEE179803C436FE7EF65003201D528440FC51975220A1AEF8E2576D1E903D79DBF744BCDDDBEA84E69745C46E669B018880A2DC80ED23022DCAA02D37E7322CB596DB9FE8B7C4046547459FA365B803A84F04A7F8011B0DDFEFD93F2D327FD52AAD47E95118D2275AAF1668E8C87B3FC805D7BB29DA839D4EE4C88AFF43DC0EBD99F1E6AF7DAEEE3839405D736CDA68338B70893D67DFE7CAB5A29212700DF7CF0BA335C93B756E1553E0B8125D7D60FAB67E3FB2FC4F87684BE2AB0D1DCAAA6834EDAB22E45F9AD177A207F0AF34D4A462273A32853CBDEDA13E4618ED01FC126A748757224FC5BAD9865899B6E33930B6CC13023222035084367A8B3477EEDCF4B0917BBDF8EC8CF3A3F9E671E1741C530C9AF456ADB139A2C55B4E5FBF77E211EBA09A38545A21B0D2287104DFCC1C791F6E7F0C53EC230647002F705516E6479000294819710BB89ABB5305B8BAD87085022F004A77C43F996D75F15950C66F6A29F187A99805783E7A60CAF5C6212FAE7A86E4CC7A37C917B97B6330704C7E858E5E12FE9E48500632B51B2DDD2A29A526ABFD4D64F7C5CF3E5F80B4FFEBCFDB73394A59AF086BD670A7573CBA3D8B6EDA9B5441FC918C77D91337A7CF21F953624023D3728FB05297D1562B18
RawData: 60820C7A06062B0601050502A0820C6E30820C6AA030302E06092A864882F71201020206092A864886F712010202060A2B06010401823702021E060A2B06010401823702020AA2820C3404820C3060820C2C06092A864886F71201020201006E820C1B30820C17A003020105A10302010EA20703050020000000A38204D6618204D2308204CEA003020105A10E1B0C444F4D41494E2E4C4F43414CA2243022A003020102A11B30191B04636966731B11646330312E646F6D61696E2E6C6F63616CA382048F3082048BA003020112A103020115A282047D0482047950F329F883A5C120C321D8F8801A44954D6ABC7BE9BABC07B4EA7083739EC57D4FAE5F373662F69DECC0B0E12DBDF9E0FA8934AFDB34F5843C2E962F40AA93C99CB1FF88950B9EA5826C856C83FA17271AEBCDC8A0E3CA34C40E3D224B78664E6C5CFDAFF12E6907BB65B05C78E3C5AE51B4AC80566A33024989A4E77751E86B684D497596BFB410F37EF7B0A530F0F1BFFC1A49FA283B1D5DCF700805ADEC68A68AF0F1512679D757D3326C9B0DED9582ACF2BBF111774AF5E00952B5D73A88AA7FF2656C985EC79EA91B75DBF5DDAD28AC9813505D25DF782AD30A78038CBF148FB159FCB8C55B8DF6D49726F18ACB09E6221AE0D32838753B79A04B82F5758145243CDBAAD5667D3C9F4D6E62C97AFE576F1F2067C296DE23B8ADAA538B006D021463F20C3F2CB0D7D901F78456BCD8E15636E5295222110C8C18BD25C44FCB246E0804D794D1156BB50B848E83813B943E41A49CFA339B2B7D31DFDC2BFC6CFC01212BA895823C0ACE411AE061D69A40351A65DB9FDF68A586D42503BB5133E7919A20FA10128B346BAED3F5A76272DF5E1A9E9BC040C816B9F279BA357D4AA447E49E640AD17954BAE45F98B0A82E1931327F2F7A8E7F72CD0C5A4D81FAAB1DDF27A510D4CF44830ADE28504F1ECB1F8C083B3F05E31829F41599CA66826BFE6E327BC891305AF5BDBE7F1CE3BDA08150B22E708D896EC6BAA8219DD45F4F857B27AA9E9A10F94C059E66A157F7F511849C1B437DC519A9496266597EF86FD099514795BF5C59E44DB42D711AFE40DCCCFF37E24DC7273522B59793FC829A0E9ADA5FCFA36F85213BEAB3D5C3EA5448AAF20213C5F94DDA9DE6DED09A4F5899C33852E6F8331B222215B818A73A97CF799189AAA0BF9859A7F20435BD43810ADC25F0275BC1040D595E3B86B647CE47DACD8D685F36413C09C29C7192C9B678E45A5FB32A6A31F4B4557932D268E343E4D672E9832318E40B487C7C8636001C860C24ED9A754A07EC65304B488F32AE77CB4243C9EC130B2C9B475D9D7F33E4FABEF4AF9910BE76552BB940AC0793585C234C9A5E99AB640A3EE729729B52891DAD2C2A12306181B9BB066914B73AB67EDECC8B7C3BD5EA45CC0E7D4A205ECA4A150A3FE69D466203AED2462898365C522ECD0FDB73EDF97A6EFCD757AE8E240AF3618DC019D686171709712F3319C86DF91A42C9FFE3CDD1B8FA6568329410258045CF703B2A3F20636A0A06C4CE05D0D53A57A967F49C0FE7C94006658ADD6899FF9AD5AD40823146DE64EEE6E36C4DCD8A27418991733FE64E7FB87957566A5533A4DE67B06C5B52947E076FEC660B07EA68BF282686C9B6B45ED4442B815CC1CA76743985A509ADF5BB79F6B4BCA5F460C6F6A8822EC7ADD5BF7B13518D5CC561F52DCAC0655D9F6942809625471FC5B8B22E9A257F947B7012C4B14917DE08C3366B138B1D23D4CD8D5DBE2A9A6399CDFA9AA0D46ADE177FC6E810A7E181DA89F11883CCC1D5C8ACE12DD47F44FA17EAAFF49C2907AE6D0CDD97161B718293CF687A2018093A4349B343880318ABB9176F2D5347C970A70C6560128B9D37C8C326899B5801BD402F1CEB55AFE2742C2F5BCEF48FA482072630820722A003020112A28207190482071514F115ACDAD14B8C5A344412BD99B7FAC1786CCA4C06936FF986F108E0872E4B723C517175A46FD7C2D0F2C985424BEABF3B07BE1AC4A9EF49AA0C58F7F028EA99D52E7E65AE544CB68E57FFBD0ECE96D3420D56683CF6B4083BBA98B9B632C1C49A8EA8EE3F52D5086C470CCE57F5D0954987CE91F02A8239D7C64BD0D2A401BEDCA94C520A3B9BD5907BE6578A50865F2F933F4B5DFB7D547499119D2C3F21BC3D86836F7F585440D8596A76CB3B8D9EA5236CB56FF1319D727FD0406D7F3B868CAD5976A596097305AEA3055A61E5C76B2C155137F195F0CF190413FB8E07DFC28F287647EA1EF13FF834630F84C7011EFDD45EB899A5410D7CBD1665192862E53C2DA7712976750763A8B86DC48740F52D015E7F0874EA0D039D567D5E4A7BCFFA7FE8771A06B5ACF2FB67D347924C4CEE5D6CCF785D5FAF0116BACECAB61C12CD554AD21984ACE71C568DF67669B33E03D6771924759E43F0C14A0C4EC558CD91FA903DC9E93D0B6926DD11E40EC526E743C8AEC021DE04CD664CC247BE93022C53AD32DB9360EAB7D9A1013E41F736807855AB9A43FC4A6389CDC936B8939D1B915A515D881911F97DE4337BFE371BB0D7FA41C9F6B196412080CDD0D391F026BDB9D620D1AD475820D1B1F89C11CF739694F9DCD7C3B44EB1ECB46EBF6ACC17D4461BB6C1E3291C3DE8A311C4BFA4059FFAAEEBB645605E9DA3ED6F97BED7723BCD3BC87975886F792EAA8CFAB23886EF84F3D52B2E21E1F01B334F80B55F5ECFA0AE08DAA2A04C819050445574608E54B863E448560D236BA66C55F6044E91704DFDC8C21A06C42952A30D8144B91F2E3654722589571DF866244B7B8E7AA6157AD1D2B5D8C432C790001511788E16077B203F8E24594834F5A42BB365EEF2CA9F8EA5316D1A43D5AD10F1B0F59635B1C21B7850B72BE0132A3BAC7A45A862B0CC9A560430B03DF66A81743A2E3F2D19366A3AA460C6047EC7C64C56AE0C1747601B5F2E3CAD15C400F1260F3272DEE08545B97A49B2FF34C89BD1D8D206F5B68A91546ABB86C4162080434DA01E64C635BF293F2053B3CB4F5B1506186C44B4462CE1279D07D39E44529479B675BC77BBB29CC34D20A0FD9924E6331C27A413DF133E47BFF2BA28F54BF6732CBF70762158172926B44312F67023842DD907E1EFE3F31D739FAD89B46EFC1E0C22E623E21A79A1732CEEF9658D602BAB4FC11153CDAD108C65F626FBC5DEBE3F8A2F390F0CC96C1002AF229E4251F4BAB199D641E489521E6D1741DA73FDB660C295CDF6E076B5D8A95C82909F8D79A429E75B7045785DE3527786F82FAF66DFD0E97C39B205305545F2A9B6F1A75B639BE3ABCB470C4BD30E0BE589B71BE7F5C61FE4CF92B8788B81ADAEDF95DB6EE6A7E7E8C6E891C9EBB5FDD0D27868F2127FCBA230E30F8DF051BF0CF4BDC454F58DF5FF8F88FC4B39A296ABF3DBDF3F5009802677DE5822876F345AFD7E9C860F3F076ACD95291781BB9945754A90C0915812A804FB16EE8130DB45494FC13310618CB509B6C097FBFD1160C4847618E459A327E2248185C96DBEFE4CBAE0B7B3509C3CD65E699C649022ADCD90CF39BCA993D61EC3FE153A2EC0A6D5CA6122DC691438B2A3BC9DADA33DE5457F581ADAB588EAE53147F42AE752CAB54BDC118ED04F5A6ED388A0CE737EEEF115CD53885194C5F701D97742A24998EF6A9F2EF2CAA291AC1E6A13F8EC262E11DFD4A0382EE918BB4D45C9E1C89C843976F4449688FEBA14068A1AEFEE924F1286E124EC574B2B0CAD580E1B80CEA3A45935D101BDC6CB6367088D0C233064025226747C9B6D5364121D968ACDC0852E2AAD1B13E7F69265E82FC017CE9A270F5AABB40C83E8F88BB12F542522905AD4854C636C24C02A3D5CEE179803C436FE7EF65003201D528440FC51975220A1AEF8E2576D1E903D79DBF744BCDDDBEA84E69745C46E669B018880A2DC80ED23022DCAA02D37E7322CB596DB9FE8B7C4046547459FA365B803A84F04A7F8011B0DDFEFD93F2D327FD52AAD47E95118D2275AAF1668E8C87B3FC805D7BB29DA839D4EE4C88AFF43DC0EBD99F1E6AF7DAEEE3839405D736CDA68338B70893D67DFE7CAB5A29212700DF7CF0BA335C93B756E1553E0B8125D7D60FAB67E3FB2FC4F87684BE2AB0D1DCAAA6834EDAB22E45F9AD177A207F0AF34D4A462273A32853CBDEDA13E4618ED01FC126A748757224FC5BAD9865899B6E33930B6CC13023222035084367A8B3477EEDCF4B0917BBDF8EC8CF3A3F9E671E1741C530C9AF456ADB139A2C55B4E5FBF77E211EBA09A38545A21B0D2287104DFCC1C791F6E7F0C53EC230647002F705516E6479000294819710BB89ABB5305B8BAD87085022F004A77C43F996D75F15950C66F6A29F187A99805783E7A60CAF5C6212FAE7A86E4CC7A37C917B97B6330704C7E858E5E12FE9E48500632B51B2DDD2A29A526ABFD4D64F7C5CF3E5F80B4FFEBCFDB73394A59AF086BD670A7573CBA3D8B6EDA9B5441FC918C77D91337A7CF21F953624023D3728FB05297D1562B18
```

```yaml
MessageType: SPNEGO NegTokenResp
Data:
  negState: accept-complete (0)
  supportedMech: MS Kerberos (1.2.840.48018.1.2.2)
  responseToken:
    MessageType: SPNEGO InitialContextToken
    Data:
      thisMech: Kerberos (1.2.840.113554.1.2.2)
      innerContextToken:
        MessageType: AP-REP (15)
        Data:
          pvno: 5
          msg-type: AP-REP (15)
          enc-part:
            etype: AES256_CTS_HMAC_SHA1_96 (18)
            kvno:
            cipher: 502B7C5E12E263CBCDF897B460F85D0D879018FE5FFA35F3A14AB8D45ACA6E102133E71CAB6B4E0D3CE8FC2EB39C02D5072EE62915FBAF61BB46803909498CD98D2D5BB04CB3000A24CD38499E6F92245CDC94AD6414ECFF368F506B2E0107A36983623148C60E310ADCECEB3745
        RawData: 02006F8188308185A003020105A10302010FA2793077A003020112A270046E502B7C5E12E263CBCDF897B460F85D0D879018FE5FFA35F3A14AB8D45ACA6E102133E71CAB6B4E0D3CE8FC2EB39C02D5072EE62915FBAF61BB46803909498CD98D2D5BB04CB3000A24CD38499E6F92245CDC94AD6414ECFF368F506B2E0107A36983623148C60E310ADCECEB3745
    RawData: 60819806092A864886F71201020202006F8188308185A003020105A10302010FA2793077A003020112A270046E502B7C5E12E263CBCDF897B460F85D0D879018FE5FFA35F3A14AB8D45ACA6E102133E71CAB6B4E0D3CE8FC2EB39C02D5072EE62915FBAF61BB46803909498CD98D2D5BB04CB3000A24CD38499E6F92245CDC94AD6414ECFF368F506B2E0107A36983623148C60E310ADCECEB3745
  mechListMIC:
RawData: A181B63081B3A0030A0100A10B06092A864882F712010202A2819E04819B60819806092A864886F71201020202006F8188308185A003020105A10302010FA2793077A003020112A270046E502B7C5E12E263CBCDF897B460F85D0D879018FE5FFA35F3A14AB8D45ACA6E102133E71CAB6B4E0D3CE8FC2EB39C02D5072EE62915FBAF61BB46803909498CD98D2D5BB04CB3000A24CD38499E6F92245CDC94AD6414ECFF368F506B2E0107A36983623148C60E310ADCECEB3745
```