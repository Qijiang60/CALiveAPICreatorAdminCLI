# Rule

This suite of commands allows you to list,add, or remove business rules in your current project. [Learn about Rules](http://docs.espressologic.com/docs/logic-designer/business-logic/learning-rules).  

## Usage
```sh
Usage: rule [options] <list|create|delete>

  Options:

    -h, --help                    output usage information
    --ruletype [type]             The type of the rule, can be: sum,formula,validation,parentcopy
    --entity_name [prefix:table]  The table, qualified with a prefix, for the rule
    --attribute_name [name]       The name of the attribute whose value is computed by the rule. Required for sum, count, formula, minimum, maximum.
    --role_name [name]            The role name - required for sum, count, minimum, maximum
    --clause [clause]             The clause - required for sum, count, minimum, maximum
    --child_attribute [name]      The name of the child attribute - required for sum, minimum, maximum
    --parent_attribute [name]     The name of the parent attribute - required for parent copy
    --expression [code]           The code for the rule - required for formula,events and validations
    --error_message [message]     The error mesaage for the rule - required for validations
    --rule_name [name]            Optional: a name for the rule. If not specified, a name will be generated.
    --comments [comments]         Optional: a comment for the rule
    --active [true|false]         Optional: whether the rule should be active, true by default
    --project_ident               The ident of a project, if other than the current project
    --ident [ident]               For delete, the ident of the rule to delete
```

***
## Rule list
    espressoadmin rule list

The `list` command shows all rules for the current project.

#### Output
	Rules
	Table               Type         Description                                                                                   Comments
	------------------  -----------  --------------------------------------------------------------------------------------------  --------------------------------------------------------------------------------------------
	demo:LineItem       formula      Derive amount as if (row.qty_ordered <= 6)  // discount (using conditional JavaScript log...  Reactive Logic is expressed in JavaScript, so you use...- conditional logic (as above),- ...
	demo:LineItem       parent copy  Derive product_price as parentcopy(product.price)                                             Parent copy means order unaffected by product price changes
	demo:PurchaseOrder  sum          Derive amount_total as sum(LineItemList.amount)                                               sum of line item amounts
	demo:customer       event        Event: var detail = {        filter: "{_id: \"32751\"}" ,        order: "",        pag...
	demo:customer       sum          Derive balance as sum(PurchaseOrderList.amount_total where paid = false)                      sum of unpaid order totals; re-used over all changes to orders (pay, insert, delete etc.)...
	demo:customer       validation   Validation return row.balance <= row.credit_limit;                                            balance cannot exceed credit limit, else throw exception
	
	# rules: 6

***
## Rule create
    espressoadmin rule create --ruletype <type> --entity_name <prefix:name> 
    	[--attribute_name <name>] [--role_name <name>] [--child_attribute <attribute>]
    	[--clause <clause>] [--expression <expression>] [--error_message <message>]
    	[--role_name <name>] [--parent_attribute <attribute>] [--active <true|false>]
    	[--comments <comments>] [--rule_name <name>]

The `create` command creates a new rule in the current project. If a name is not specified,
one will be generated.

The `ruletype` parameter must be one of the following values, with the corresponding parameters:

### `sum`
A sum must specify an `attribute_name` to hold the value of the sum, a `role_name` and a 
`child_attribute`. It may also specify a `clause` to qualify the sum.

### `count`
A count must specify an `attribute_name` to hold the value of the sum, and a `role_name`.
It may also specify a `clause` to qualify the count.

### `formula`
A formula must specify an `attribute_name` to hold the value of the formula, and an `expression`
with the code.

### `parentcopy`
A parent copy must specify an `attribute_name` to hold the value, a `role_name` pointing to the
parent table, and a `parent_attribute` in that parent table.

### `validation` and `commitvalidation`
A validation must specify an `expression` with the code for the validation. It may also specify
an `error_message`.

### `event`, `earlyevent` and `commitevent`
An event must specify an `expression` with the code.

### `minimum` and `maximum`
A minimum or maximum must specify an `attribute_name` to hold the value of the minimum/maximum,
a `role_name` and a `child_attribute` to be watched. It may also specify a `clause` to qualify the sum.

### `managedparent`
A managed parent must specify a `role_name` to the parent table. It may also specify an
`expression`, whose code will be executed when the managed parent is executed.

***
## Rule delete

	espressoadmin rule delete [--ident <ident> | --rule_name <name>]

The `delete` command deletes the specified rule. You can specify the rule either by its ident or its name.  

Visit our documentation page for more [details.](http://docs.espressologic.com/docs/logic-designer/business-logic)