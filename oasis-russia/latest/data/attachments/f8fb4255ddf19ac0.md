# Page snapshot

```yaml
- generic [active] [ref=e1]:
  - generic [ref=e4]:
    - generic [ref=e5]:
      - generic [ref=e6]:
        - text: OAZIS
        - generic [ref=e7]: Welcome
      - generic [ref=e8]:
        - generic [ref=e10]:
          - generic [ref=e12]: Email
          - textbox "Email" [ref=e13]:
            - /placeholder: Enter the email
        - generic [ref=e15]:
          - generic [ref=e16]: Password
          - generic [ref=e17]:
            - textbox "Password eye-invisible" [ref=e18]:
              - /placeholder: "***********"
            - img "eye-invisible" [ref=e20] [cursor=pointer]:
              - img [ref=e21]
        - button "Sign In" [disabled] [ref=e25]:
          - generic [ref=e27]: Sign In
    - generic [ref=e28]: Version 1.3.40
  - tooltip "* less than 8 characters * no uppercase Latin letters * no lowercase Latin letters * no digits * do not use Cyrillic characters" [ref=e31]:
    - list [ref=e33]:
      - listitem [ref=e34]:
        - paragraph [ref=e35]: "* less than 8 characters"
      - listitem [ref=e36]:
        - paragraph [ref=e37]: "* no uppercase Latin letters"
      - listitem [ref=e38]:
        - paragraph [ref=e39]: "* no lowercase Latin letters"
      - listitem [ref=e40]:
        - paragraph [ref=e41]: "* no digits"
      - listitem [ref=e42]:
        - paragraph [ref=e43]: "* do not use Cyrillic characters"
```