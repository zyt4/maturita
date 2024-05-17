# Služby Azure pro zajištění identity, přístupu a zabezpečení

## Azure Active Directory, Azure Active Directory Domain Services

AAD domain services = pro kompatibilitu se starší verzí on-prem AD. Je možné je propojit přes AD connect

AAD je nově přejmenováné ne Azure Entra ID
- umožňuje přihlášení i k ostaním microsoft aplikacím
- pomáhá s nasazením on-prem AD
- správa identit přes celý cloud
- umí monitorovat přihlášení z nestandartních míst

Poskytuje služby:
- ověření (více faktor)
- SSO
- správa aplikací v cloudu a on-prem
- správa zařízení
- zpětně kompatibilní s AD

## Možnosti ověření ve službě Azure entra id

Možnosti ověření:
- heslo
- MFA
- bezheslové FIDO2 klíče
- Microsoft Hello

Entra ID podporuje přihlášení pomocí externích identit, např při kolaboraci s externí firmou, přihlášení uživatelů k web aplikacím...

externí identiti:
- B2B colaboration = (business to business), použijí svou preferovanou identity, třeba pracovní nebo google účet
  - v AD se pak jeví hako host users
- B2B direct connect = vytvoří navzájem důvěrný vztah s entra ID jiné firmy (pouze pokud oba mají entra id)
  - pro přihlášení si pak tahá informace z jejich serveru
- B2C (business to costumer) = poublikování SaaS aplikací, dá se použít entra id pro správu přístupu a uživatelů, přihlášení přes jiné účty(google, facebook...)

## Podmíněný přístup, přístup na základě rolí

Entra Id dokáže sledovat a vyžadovat ověření na základě různých faktorů:
- zařízení ze kterého se přihlašujeme
- naše lokace
- jak dlouho jsme se nepřihlásili
- k čemu se chceme přihlásit
- ...

Na základě těchto údajů pak může vyžadovat dodatečné ověření(MFA...), neob zablokovat přístup úplně

Role based access control = stejně jako na windows
- nadefinujeme si role = reader, contribuitor, admin...
- přístupy a oprávnění k prostředkům pak přiřazujeme rolím, ne uživatelům
- uživatelům se pak přiřazují až role
- role se dají definovat v rozmezí: předplatného, resource groupy, management groupy, resourcu
- nižší prvek dědí role z prvnku vyššího
- funguje na principu povolení přístupu = co neni povoleno to je zakázáno

## Model Zero trust a defense in depth

Zero trust:
- všechny zařízení bere jako neznámé a potenciálně nebezpečné
- vždy při připojení je třeba se ověřit, a to i při přístupu k různým službám
- princip nejmenšího potřebného oprávnění
- jedná se převážně o teorii, jelikož uživatele by to nevydrželi a trvalo by to dlouho

defense in depth
- obrana probíhá ve více vrstvách
- physical security
  - edentity access
    - perimetr security
      - network
        - compute
          - application
            - data
              - MFA
                - firewall
                  - nastavení systému
                    - bezpečný kód
                      - CIA




