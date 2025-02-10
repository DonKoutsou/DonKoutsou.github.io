---
title: "Inventory Tutorial"
excerpt_separator: "<!--more-->"
categories: Tutorial
tags: Tutorial Godot
permalink: "/tutorials/InventoryTutorial"
---

GIT REPO : [GitLink](https://github.com/DonKoutsou/Inventory_Tutorial)

Ένα από τα συχνότερα συστήματα που θα βρει κάποιος μέσα στα video game είναι το inventory system, αλλιώς η τσάντα. Έχει πολλές μορφές, από απλές λίστες αντικειμένων όπως τα Pokemon, μέχρι κάτι πιο ανεπτυγμένο σαν το DayZ που κοντεύει τα όρια του τέτρις. 
Αγαπημένο σύστημα από πολλούς, αλλά και μισητό από πολλους άλλους. Άτομα επιλέγουν ορισμένες φορές να το αγνοούν λόγω άβολων χειρισμών ή άχρηστης πολυπλοκότητας.

Ξεκινώντας, κάποιος να φτιάξει ένα παιχνίδι, πολύ πιθανό να θέλει να δημιουργήσει ένα τέτοιο σύστημα με την πεποίθηση να εμβαθύνει την πολυπλοκότητα του παιχνιδιού ή απλά επειδή το είδος του παιχνιδιού να το απαιτεί. Ας εξετάσουμε πώς θα μπορέσει να σχεδιαστεί ένα τέτοιο σύστημα.

Για να ξεκινήσουμε θα πρέπει να πάρουμε κάποιες αποφάσεις. Οι αποφάσεις αυτές θα μας βοηθήσουν όταν πλέον ξεκινήσουμε να προγραμματίζουμε το σύστημα. Ο προγραμματισμός με έλλειψη σχεδίου είναι επικίνδυνος για διάφορους λόγους. Αν και το σύστημα που θα φτιάξουμε σήμερα είναι αρκετά απλό, η σύνθεση ενός γενικού πλάνου είναι καλό συνήθειο. Δεν χρειαζόμαστε μια βίβλο. Ακόμα και μερικά bullet point μπορούν να κάνουν τη δουλειά.
{: .notice}

Αυτά που έχω ετοιμάσει για το σύστημά μου είναι 
Η τσάντα θα έχει όριο βάρους. Αυτό μας δείχνει ότι τα αντικείμενα θα πρέπει να έχουν βάρος που θα ορίζεται ανά αντικείμενο. Βάζει όριο στα πόσα αντικείμενα μπορεί ο παίκτης να κουβαλήσει. Αναγκάζει τον παίκτη να σκεφτεί τι θα πάρει μαζί του και τι θα αφήσει πίσω. 
Ορισμένα αντικείμενα να έχουν περιεχόμενα μέσα τους.  Αυτά τα περιεχόμενα πιθανόν αλλάζουν το βάρος του αντικειμένου Ένα μπουκάλι νερό, ένα μπιτόνι βενζίνη κτλ 
Η χρήση αυτών των αντικείμενων θα ορίζεται από το ίδιο το παιχνίδι και δεν έχει κάποια σημασία για το inventory. π.χ.ι για κάποιο survival ή puzzle. Το inventory θα πρέπει μόνο να μας δίνει πρόσβαση στις πληροφορίες και τη δυνατότητα να τις παραποιούμε Δηλαδή να τις καταναλώνουμε ή να τις γεμίζουμε
Το interface θα είναι μια απλή λίστα. Δεν χρειαζόμαστε κάτι πιο extreme. 
Το κάθε αντικείμενο μέσα στη λίστα θα μπορεί να διαλεχτεί και όταν ένα αντικείμενο διαλέγεται θα μας δίνει επιλογές. Οι επιλογές είναι οι εξής, 
χρήση ή αλλιώς use. Όταν ο παίκτης κοιτάει κάτι στο οποίο μπορεί να χρησιμοποιηθεί το αντικείμενο, το αντικείμενο καταναλώνεται και γίνεται η χρήση. 
* Επιθεώρηση ή αλλιώς inspect. Άνοιγμα παραθύρου με περιγραφή αντικειμένου που προβάλει visual 2D ή 3D. 
* Κατανάλωση ή consume
* άφησε ή drop
* άκυρο cancel

Έχοντας αυτά τα στοιχεία μαζί μέρα, πλέον γνωρίζουμε 
πώς να ορίσουμε ένα αντικείμενο και πώς αυτό θα επιδρά με το σύστημα 
πόσο UI θα χρειαστούμε 
πώς τα εξωτερικά συστήματα θα επικοινωνούν με το Inventory

Θα ξεκινήσουμε το προγραμματισμό ορίζοντας τι είναι ένα αντικείμενο. Δημιουργούμε τους πρώτους φακέλους και το script που θα ορίζει το αντικείμενο.
```gdscript
extends Resource

class_name Item

@export var _ItemName : String
@export var _ItemDescription : String
@export var _ItemWeight : float
@export var _ItemIcon : Texture

func GetItemName() -> String:
	return _ItemName

func GetItemDescription() -> String:
	return _ItemDescription

func GetItemWeight() -> float:
	return _ItemWeight

func CanStack() -> bool:
	return true
```
Το script του Item θα συνταχθεί κάπως έτσι για αρχή και μετά μπορούμε να το επεκτείνουμε.
{: .notice}
Για αρχή το script θα κάνει inherit από την κλάση Resource. Ο λόγος αυτού είναι οτι αυτή η κλάση θα υπάρχει για αναπαριστά δεδομένα και τίποτε άλλο, όπως αναφέρεται και στα doc τα resources είναι data containers, οταν κάποιο στιγμή θελήσουμε να αναπαραστήσουμε ένα αντικείμενο σε μιά σκηνή τότε θα δημιουργήσουμε μια άλλη κλάση που θα κάνει Inherit από node.
Τα “underscore” ( _ ) που τοποθετούνται πριν τα ονόματα των variable είναι για δική μου εξυπηρέτηση, έτσι σημαδεύω τα private variable μιας και το gdScript δεν έχει private variable.
Για λόγους οργάνωσης θέλω να σιγουρευτώ οτι η πρόσβαση/αλλαγή δεδομένων θα γίνεται μέσω function, για αυτόν τον λόγο έχω στήσει και τα πρώτα μου Getter, δηλαδή function που θα μου επιστρέφουν τα variable αυτής της κλάσης.
Το function CanStack θα μας χρησιμεύσει αργότερα. Θα μπορέσουμε να κάνουμε override αυτό το function και έτσι να επιτράπουμε ή να αποτρέπουμε σε ορισμένα αντικείμενα να κάνουν stack μέσα στο inventory. Όταν λέμε stack εννοούμε οτι μέσα στο Inventory θα μας δείχνει οτι έχουμε 4χ από ένα αντικείμενο.
Μία πολύτιμη δυνατότητα της χρήσης των Getter είναι οτι σε κλάσεις που επεκτείνουν το Item θα μπορούμε να κάνουμε override την συμπεριφορά τους και να προσθέσουμε έξτρα πληροφορίες.
{: .notice}
Για παράδειγμα, χτίζουμε την κλάση ContainerItem η οποία θα αναπαριστά τα αντικείμενα που θα έχουν περιεχόμενο (μπουκάλι με νερό) όπως ορίσαμε και στο αρχικό design.
```gdscript
extends Item

class_name ContainerItem

@export var _ContentName : String
@export var _Capacity : float
@export var _CurrentContetAmmount : float

func GetItemDescription() -> String:
	return "{0} | {1}/{2} of {3}".format([_ItemDescription, _CurrentContetAmmount, _Capacity, _ContentName])

func GetContentName() -> String:
	return _ContentName

func GetContentCapacity() -> float:
	return _Capacity

func GetContentAmmount() -> float:
	return _CurrentContetAmmount

func GetItemWeight() -> float:
	return _ItemWeight + _CurrentContetAmmount

func CanStack() -> bool:
	return false
```
Πρώτον στο Description θα προσθέσουμε και τα περιεχόμενα του αντικειμένου.
Δεύτερον στο ItemWeight θα προσθέσουμε και το βάρος του περιεχομένου, προς το παρόν ο ορισμός του περιεχομένου είναι πολύ βασικός, αλλά θα τον εξελίξουμε παρακάτω.

Μπορούμε πλέον να δημιουργήσουμε resource με βάση τις κλάσης που μόλις σχεδιάσαμε και να αρχίσουμε να δημιουργούμε τα αντικείμενα που μπορεί να θέλαμε στο παιχνίδι μας.

<img src="/assets/images/ItemCreation.jpg" alt="Alt text" width="600" />

Αρχίζοντας να κατασκευάζουμε την κλάση του Inventory χτίζουμε τα βασικά στοιχεία για την διαχείριση του βάρους.
Inventory script
```gdscript
extends Resource

class_name Inventory

@export var MaxWeight : float = 40

var _CurrentWeight : float = 0

var _InventoryContents : Array[InventoryItemContainer]
```
ItemContainer script
```gdscript
extends Resource

class_name InventoryItemContainer

var _StoredItem : Item
var _Ammount : int = 0

signal OnAmmountUpdated(NewAmmount : int)
signal OnContainerEmptied

func RegisterItem(It : Item) -> void:
	_StoredItem = It

func UpdateAmm(Amm : int) -> void:
	_Ammount += Amm
	OnAmmountUpdated.emit(_Ammount)

func GetAmmount() -> int:
	return _Ammount
	
func GetContainedItem() -> Item:
	return _StoredItem
```
To inventory θα μπορεί να τοποθετηθεί μέσα σε οποιαδήποτε κλάση που θα χρειαστεί να έχει inventory, από χαρακτήρες, μέχρι chest.
Θέλοντας να φτιάξουμε μια λίστα από αντικείμενα που να μας δίνει την δυνατότητα να μην κάνουμε stack αντικείμενα το Dictionary είναι εκτός, από την στιγμή που δεν θα μπορέσουμε να βάλουμε 2 ίδα key.
Για αυτόν τον λόγο θα φτιάξουμε μια καινούρια κλάση που θα δουλέψει σαν ένα μεμονωμένο container για κάθε αντικείμενο που θα θέλουμε να βάλουμε στο inventory.
Αυτή η κλάση θα είναι σχετικά απλή και θα περιέχει μόνο τα στοιχεία του αντικειμένου που περιέχει,ι την ποσότητα και ένα signal για να ενημερώνει το UI.

```gdscript
extends Resource

class_name Inventory

@export var MaxWeight : float = 40


var _CurrentWeight : float = 0

var _InventoryContents : Array[InventoryItemContainer]

signal ContainerCreated(Cont : InventoryItemContainer)
signal OnItemUsed(Cont : InventoryItemContainer)
signal OnItemDropped(It : Item)
signal OnWeightChanged(NewW : float)

func AddItemToInventory(It : Item) -> void:
	if (It.CanStack()):
		var ItemContainer = GetContainerForItem(It)
		if (ItemContainer == null):
			ItemContainer = InventoryItemContainer.new()
			ItemContainer.RegisterItem(It)
			_InventoryContents.append(ItemContainer)
			ContainerCreated.emit(ItemContainer)
		ItemContainer.UpdateAmm(1)
		OnItemAdded(It)
	else:
		var ItemContainer = InventoryItemContainer.new()
		ItemContainer.RegisterItem(It)
		ItemContainer.UpdateAmm(1)
		_InventoryContents.append(ItemContainer)
		ContainerCreated.emit(ItemContainer)
		OnItemAdded(It)

func OnItemAdded(It : Item) -> void:
	_CurrentWeight += It._ItemWeight
	OnWeightChanged.emit(_CurrentWeight)


func HasItem(It : Item) -> bool:
	for g in _InventoryContents:
		if (g.GetContainedItem().resource_path == It.resource_path):
			return true
	return false

func GetContainerForItem(It : Item) -> InventoryItemContainer:
	for g in _InventoryContents:
		if (g.GetContainedItem().resource_path == It.resource_path):
			return g
	return null
```
Έχοντας το container έτοιμο μπορούμε να συνεχίσουμε την δημιουργία του inventory.
Θα δημιουργήσουμε ένα function που θα χρησιμοποιούμε για να τοποθετούμε αντικείμενα μέσα στο inventory και μερικά “βοηθητικά” function για να ελέγχουμε την κατάσταση του inventory.
Αφού δεν βάλαμε κάποιο “check” μέσα στο AddItemToInventory function θα πρέπει να θυμόμαστε να ελέγξουμε αν το αντικείμενο θα χωρέσει χρησιμοποιώντας το CanFitItem function πριν προσπαθήσουμε να βάλουμε κάποιο αντικείμενο.
Τέλος 2 signal που θα χρησιμοποιήσουμε για να ενημερώνουμε το UI.

Θα εμβαθύνουμε λίγο στο function AdditemToInventory για να καταλάβουμε πως λειτουργεί.
Ξεκινάμε τσεκάροντας αν το αντικείμενο μπορεί να μπει σε στοίβα
```gdscript
if (It.CanStack()):
```
Αν ναι θα πάμε να δούμε αν υπάρχει κάποιο έτοιμο container χρησιμοποιώντας το GetContainerForItem function, για να τοποθετήσουμε το αντικείμενο μέσα, αν το function επιστρέχει null, σημαίνει οτι δεν υπάρχει και έτσι θα πρέπει φτιάξουμε ένα καινούριο, θα πρέπει να θυμηθούμε να τοποθετησουμε το container στην λίστα μας _InventoryContents

```gdscript
var ItemContainer = GetContainerForItem(It)
		if (ItemContainer == null):
			ItemContainer = InventoryItemContainer.new()
      ItemContainer.RegisterItem(It)
			_InventoryContents.append(ItemContainer)
		ItemContainer.UpdateAmm(1)
		OnItemAdded(It)
```
Αν το αντικείμενο δεν μπορεί να μπει σε στοίβα τότε φτιάχνουμε ένα καινούργιο container.
```gdscript
else:
		var ItemContainer = InventoryItemContainer.new()
		ItemContainer.RegisterItem(It)
		ItemContainer.UpdateAmm(1)
		_InventoryContents.append(ItemContainer)
		OnItemAdded(It)
```
**UI**
Με το inventory σε καλή κατάσταση μπορούμε να ξεκινήσουμε να στήνουμε το UI.
Πριν ξεκινήσουμε το UI ας χτίσουμε πρώτα μερικά αντικείμενα για να μπορούμε να να τεστάρουμε το σύστημα.
Μπορούμε να προμηθευτούμε μερικά εικονίδια για να ξεκινήσουμε από εδώ.
[Game Icons](https://game-icons.net/)

```gdscript
extends Resource

class_name Item

@export var _ItemName : String
@export var _ItemDescription : String
@export var _ItemWeight : float
@export var _ItemIcon : Texture

func GetItemName() -> String:
	return _ItemName

func GetItemDescription() -> String:
	return _ItemDescription

func GetItemWeight() -> float:
	return _ItemWeight

func CanStack() -> bool:
	return true
	
func GetItemIcon() -> Texture:
	return _ItemIcon
```

Θα προσθέσουμε το εικονίδιο στο script του Item για να μπορέσουμε να το κάνουμε configure.
Και θα δημιουργήσουμε το πρώτο αντικείμενο

<img src="/assets/images/ItemRockCondif.jpg" alt="Alt text" width="600" />

Θα ξεκινήσουμε το UI φτιάχνοντας το “Container”.
{: .notice}
<img src="/assets/images/ItemContainerH.jpg" alt="Alt text" width="600" />
<img src="/assets/images/ItemContainerVisual.jpg" alt="Alt text" width="600" />

Το container θα χρησιμοποιείται για την αναπαράσταση κάθε στήβας στο inventory.
Κάθε φορά που ένα “InventoryItemContainer” θα δημιουργείται στο inventory, θα δημιουργούμε και την αναπαράστασή του στο UI.
Η σύνθεση του είναι απλή, ένα panel container για background, και 3 αντικείμενα στοιχισμένα, το εικονίδιο, η ποσότητα και το όνομα του αντικειμένου.

Το script του θα είναι κάπως έτσι. 
{: .notice}
```gdscript
extends PanelContainer

class_name InventoryUIContainer

@export var Icon : TextureRect
@export var ItemAmmount : Label
@export var ItemName : Label

func RegisterContainer(Cont : InventoryItemContainer) -> void:
	Cont.connect("OnAmmountUpdated", OnAmmountUpdated)
	ItemAmmount.text = var_to_str(Cont.GetAmmount())
	ItemName.text = Cont.GetContainedItem().GetItemName()
	Icon.texture = Cont.GetContainedItem().GetItemIcon()

func OnAmmountUpdated(Amm : int) -> void:
	ItemAmmount.text = var_to_str(Amm)
```
Θα χρειαστούμε ένα function που θα χρησιμοποιούμε για να κάνουμε “inject” τα data από την στοίβα που θα δημιουργηθεί στο inventory και θα θέλουμε να αναπαραστήσουμε.
Θα συνδεθούμε στο Signal που στήσαμε στο InventoryItemContainer για να ενημερώνουμε το UI για αλλαγές στην ποσότητα αυτής την στήβας.

Το inventory screen θα είναι και αυτό απλό, θέλουμε απλά να δημιουργήσουμε την λίστα στην οποία θα μπορούν να στοιχιστούν τα UI element που φτιάξαμε προηγουμένως.

<img src="/assets/images/InventoryScreenUI.jpg" alt="Alt text" width="600" />
 
<div style="display: flex; justify-content: space-between;">
  <div style="flex: 1; text-align: center;">
	Η λίστα άδεια
  </div>
  <div style="flex: 1; text-align: center;">
	Kαι με μερικά element
  </div>
</div>
<div style="display: flex; justify-content: space-between;">
  <div style="flex: 1; text-align: center;">
    <img src="/assets/images/InventoryScreenVisualEmpty.jpg" alt="Image 1" style="width: 150px; height: auto;">
  </div>
  <div style="flex: 1; text-align: center;">
    <img src="/assets/images/InventoryScreenVisualFilled.jpg" alt="Image 2" style="width: 150px; height: auto;">
  </div>
</div>

Το script του Inventory screen θα είναι κάπως έτσι.

```gdscript
extends Control

class_name InventoryScreen

@export var WeightText : Label
@export var ContainerPlacement : Control
@export var InventoryContainerScene : PackedScene
@export var Inv : Inventory

func _ready() -> void:
	Inv.connect("ContainerCreated", OnContainerCreated)
	Inv.connect("OnWeightChanged", OnWeightUpdated)
	OnWeightUpdated(Inv.GetCurrentWeight())

func OnContainerCreated(Cont : InventoryItemContainer) -> void:
	var UIContainer = InventoryContainerScene.instantiate() as InventoryUIContainer
	UIContainer.RegisterContainer(Cont)
	ContainerPlacement.add_child(UIContainer)

func OnWeightUpdated(NewW : float) -> void:
	WeightText.text = "Weight : {0} / {1}".format([NewW, Inv.MaxWeight])

func _input(event: InputEvent) -> void:
	if (event.is_action_pressed("Inventory")):
		visible = !visible
```
Χρησιμοποιούμε το @export για να κάνουμε configure τα διαφορετικά UI element, την σκηνή που φτιάξαμε για να αναπαραστήσουμε το container και τέλος το Inventory.

<img src="/assets/images/PlayerInventory.jpg" alt="Alt text" width="600" />

Το Inventory είναι resource οπότε μπορούμε να το διμιουργήσουμε μέσα στο project μας και να το τοποθετήσουμε όπου το χρειαζώμαστε.
Μέσα στο _ready function θα συνδεθούμε στα signal που ορίσαμε στο Inventory και θα κάνουμε update το value του weight.
Ένα function που θα δέχεται τα input και θα κρύβει ή θα δείχνει το Inventory
Και άλλο ένα το οποίο θα χρησιμοποιείται από το inventory όταν κάποια καινούργια στοίβα δημιουργείται.


<img src="/assets/images/CondifuredInvUI.jpg" alt="Alt text" width="600" />
Σιγουρευόμαστε ότι έχουμε κάνει configure τα πάντα και συνεχίζουμε.
{: .notice}

Θα κάνουμε configure το Input που βάλαμε στο InventoryScreen για να ανοιγοκλείνει το UI και μπορούμε πλέον να τεστάτουμε το Inventory. Παίζοντας την σκηνή του InventoryScreen, μπορούμε να δούμε οτι το UI αντιδρά στο Input και οτι το weight κάνει update με βάση το τι κάναμε configure στο PlayerInventory.tres

Η βάση του Inventory έχει χτιστεί πλέον και μπορούμε να αρχίσουμε να το συνδέουμε σε ένα παιχνίδι.
Θα χτίσω κάτι γρήγορο για να το τεστάρω.


<img src="/assets/images/ItemH.jpg" alt="Alt text" width="600" />
Θα χρειαστούμε μια αναπαράσταση του αντικειμένου στον κόσμο, αφού θα φτιάξουμε κάτι γρήγορο, το πιό εύκολο είναι το 2D.
Η σύνθεσή του θα είναι απλή. Ένα sprite για να βάζουμε το texture του, θα χρησιμοποιήσουμε collision για να βλέπουμε πότε είμαστε πάνω από ένα αντικείμενο για να το τοποθετήσουμε στην τσάντα οπότε ο χαρακτήρας θα χρειαστεί και ένα Area2D.
```gdscript
extends Node2D

class_name Item2D

@export var ItemSprite : Sprite2D

var ItemResource : Item

func SetItem(It : Item) -> void:
	ItemResource = It
	ItemSprite.texture = It.GetItemIcon()

func GetItemResource() -> Item:
	return ItemResource
```

<img src="/assets/images/CharacterH.jpg" alt="Alt text" width="600" />
Ένας απλός χαρακτήρας, ένα sprite και το area2D για να ανιχνεύει τα αντικείμενα. Στό script θα του δώσουμε πρόσβαση στο Inventory,  ο χαρακτήρασ θα χρειαστεί κάποια απλά input και την λογική για το collision, οταν το area2D έχρεται σε επαφή με ένα άλλο Area2D κοιτάμε αν είναι αντικείμενο.
Αν ναι το βάζουμε στο inventory και διαγράφουμε την 2D αναπαράσταση του.
```gdscript
extends Node2D

class_name Character

@export var Speed : float
@export var Inv : Inventory

#καθορίζουμε την κήνηση του παίχτη με βάση τα Input
func _physics_process(delta: float) -> void:
	if (Input.is_action_pressed("MoveUp")):
		position.y -= Speed
	if (Input.is_action_pressed("MoveDown")):
		position.y += Speed
	if (Input.is_action_pressed("MoveLeft")):
		position.x -= Speed
	if (Input.is_action_pressed("MoveRight")):
		position.x += Speed
	
func _on_item_area_area_entered(area: Area2D) -> void:
	if (area.get_parent() is Item2D):
		var It = area.get_parent() as Item2D
		if (Inv.CanFitItem(It.GetItemResource())):
			Inv.AddItemToInventory(It.GetItemResource())
			It.queue_free()
	else : if (area.get_parent() is Interactable):
		Interactables.append(area.get_parent())
```
Θα φτιάξουμε μια σκηνή που όλα θα υπάρξουν μαζί και την ονομάζουμε World (Κόσμος).

<img src="/assets/images/WorldH.jpg" alt="Alt text" width="600" />

Το script του κόσμου θα έχει μια βασική λογική για να έχουμε ένα καλό περιβάλλον να τεστάρουμε το inventory. Θα το βάλουμε να μας κάνει spawn 10 αντικείμενα και να τα μοιράσει γύρο.

```gdscript
extends Control

class_name World

@export var ItemScene : PackedScene

@export var ItemsToSpread : Array[Item] = []

func _ready() -> void:
	var Viewportsize = get_viewport_rect().size
	for g in 10:
		var ItRes = ItemsToSpread.pick_random()
		var It = ItemScene.instantiate() as Item2D
		It.SetItem(ItRes)
		add_child(It)
		It.position = Vector2(randf_range(0, Viewportsize.x), randf_range(0, Viewportsize.y))
```

Όλα φαίνεται να λειτουργούν σωστά, τα αντικείμενα αφαιρούνται από τον κόσμο, εισέρχονται στο inventory, το καθένα στην δικιά του στοίβα και το UI ενημερώνεται σωστά.

<img src="/assets/images/PickUpGif.gif" alt="Alt text" width="600" />

Με το inventory πλήρως λειτουργικό θα κάνουμε implement τα τελευταία κομμάτια του design.
-Αντικείμενα με περιεχόμενα (μπουκάλι νερό)
-Menu επιλογών για την κάθε στοίβα.

Πρώτα θα κάνουμε την λίστα επιλογών. Θα χρειαστούμε για αρχή να μπορούμε να διαλέγουμε τα διαφορετικά UI container τις κάθε στήβας
Για αρχή θα φτιάξουμε την λίστα με τις επιλογές. Χρειαζόμαστε μια απλή λιστα με κουμπιά που θα μας λέει πιο αντικείμενο έχουμε διαλεγμένο και να μας δίνει όλας τις πιθανές επιλογές.
<img src="/assets/images/ItemOptions.jpg" alt="Alt text" width="600" />

Θα φτιάξουμε ένα panel container για background και θα στιχίσουμε κατακόρυφα το label για τον όνομα του αντικειμένου με όλα τα κουμπιά. Θα φτιάξουμε μια 
 Κλάση που θα κρατάει όλα τα event για τα κουμπιά και θα ενημερώνει το UI οταν πατιέται κάποια επιλογή. Θα συνδέσουμε όλα τα “pressed” signal από τα κουμπιά και θα προγραμματίσουμε την λογική.

```gdscript
extends PanelContainer

class_name UIItemOptions

@export var ItemNameLabel : Label

signal OnItemUsed()
signal OnItemInspected()
signal OnItemDroped()
signal OnItemConsumed()

func _ready() -> void:
	visible = false

func SetSelectedItem(It : Item) -> void:
	visible = true
	ItemNameLabel.text = It.GetItemName()

func _on_use_button_pressed() -> void:
	OnItemUsed.emit()


func _on_inspect_button_pressed() -> void:
	OnItemInspected.emit()


func _on_consume_button_pressed() -> void:
	OnItemConsumed.emit()


func _on_drop_button_pressed() -> void:
	OnItemDroped.emit()


func _on_cancel_button_pressed() -> void:
	visible = false
```
Θα περνάμε το αντικείμενο που μόλις διαλέχτηκε για να βάζουμε το όνομα του στο label. Θα δημιουργήσουμε όλα τα signal για το κάθε κουμπί και θα τα τοποθετήσουμε στα σωστά function που έρχονται από τα κουμπιά. Η λογική του τι θα γίνεται όταν πατάμε το κάθε κουμπί θα μπει στο InventoryScreen που θα επικοινωνεί κατευθείαν με το Inventory system.

Θα χρειαστούν κάποιες αλλαγές στα UI που φτιάξαμε μέχρι τώρα.
{: .notice}

<img src="/assets/images/InventoryContainerButton.jpg" alt="Alt text" width="600" />

Στο UIInventoryContainer θα βάλουμε ένα κουμπί και θα περάσουμε το event “pressed” στην κλάση η οποία με την σειρά της θα κάνει emit το καινούριο signal που θα τοποθετήσουμε στο οποίο θα κάνουμε connect από το InventoryScreen.
Μία ακόμη αλλαγή είναι ότι κρατάμε referance του InventoryItemContainer για να δημιουργήσουμε το function “GetContainedItem”, αυτή την πληροφορία θα την χρειαστούμε οταν το συγκεκριμένο container διαλεχτεί.
Εδώ πρέπει να σιγουρευτούμε οτι όλα τα υπόλοιπα element στην ιεραρχία αυτής της σκηνής έχουν το “mouse filter” τους στο ignore έτσι ώστε το κουμπί που βάλαμε να μπορεί να πατηθεί.


```gdscript
extends PanelContainer

class_name InventoryUIContainer

@export var Icon : TextureRect
@export var ItemAmmount : Label
@export var ItemName : Label

var _ItemCont : InventoryItemContainer

signal OnContainerSelected

func RegisterContainer(Cont : InventoryItemContainer) -> void:
	Cont.connect("OnAmmountUpdated", OnAmmountUpdated)
	Cont.connect("OnContainerEmptied", DeleteSelf)
	_ItemCont = Cont
	ItemAmmount.text = var_to_str(Cont.GetAmmount())
	ItemName.text = Cont.GetContainedItem().GetItemName()
	Icon.texture = Cont.GetContainedItem().GetItemIcon()

func OnAmmountUpdated(Amm : int) -> void:
	ItemAmmount.text = var_to_str(Amm)

func GetContainer() -> InventoryItemContainer:
	return _ItemCont

func _on_select_button_pressed() -> void:
	OnContainerSelected.emit()
```

Στην  γραμμή 22 συνδεόμαστε στο signal και κάνουμε “bind” το ίδιο το container ώστε να μας επιστρέψει όταν καλεστεί το “OnItemContainerSelected”.
Όταν γίνει αυτό πηγαίνουμε στο InventoryOption panel που φτιάξαμε και του δίνουμε το αντικείμενο του container χρησιμοποιώντας το “GetContainedItem” function που μόλις φτιάξαμε.
Σιγουρευόμαστε οτι αποθηκεύουμε το reference του διαλεγμένου container για να το χρησιμοποιήσουμε όταν μία επιλογή επιλεχθεί.
```gdscript
extends Control

class_name InventoryScreen

@export var WeightText : Label
@export var ContainerPlacement : Control
@export var ItemOptions : UIItemOptions
@export var InventoryContainerScene : PackedScene
@export var Inv : Inventory
@export var InventoryDescriptorScene : PackedScene

var SelectedContainer : InventoryItemContainer

func _ready() -> void:
	Inv.connect("ContainerCreated", OnContainerCreated)
	Inv.connect("OnWeightChanged", OnWeightUpdated)
	OnWeightUpdated(Inv.GetCurrentWeight())

func OnContainerCreated(Cont : InventoryItemContainer) -> void:
	var UIContainer = InventoryContainerScene.instantiate() as InventoryUIContainer
	UIContainer.RegisterContainer(Cont)
	ContainerPlacement.add_child(UIContainer)
	UIContainer.connect("OnContainerSelected", OnItemContainerSelected.bind(UIContainer))

func OnWeightUpdated(NewW : float) -> void:
	WeightText.text = "Weight : {0} / {1}".format([NewW, Inv.MaxWeight])

func _input(event: InputEvent) -> void:
	if (event.is_action_pressed("Inventory")):
		visible = !visible

func OnItemContainerSelected(ItemContainer : InventoryUIContainer) -> void:
	SelectedContainer = ItemContainer.GetContainer()
	ItemOptions.SetSelectedItem(SelectedContainer.GetContainedItem())
```

Τώρα που το menu είναι στημένο θα αρχίσουμε να ορίζουμε μία μία τις διαφορετικές επιλογές που έχουμε βάλει στο design.
Το OnItemUsed θα περιμένει λίγο γιατι θα πρέπει να ορίσουμε κάτι για να χρησιμοποιηθεί επάνω πρώτα.
Το OnItemInspected θα χρειαστεί UI
Το OnItemConsumed θα χρειαστεί να ορίσουμε μερικά stat επάνω στον χαρακτήρα οπότε είναι και αυτό λίγο δουλειά.
Το OnItemDroped είναι το πιο εύκολο για τώρα. Θα χρειαστεί να κάνουμε κάποιες αλλαγές στο inventory για να μπορούμε να αφαιρέσουμε πράματα.

```gdscript
func OnItemUsed() -> void:
	pass

func OnItemInspected() -> void:
	pass

func OnItemConsumed() -> void:
	pass

func OnItemDroped() -> void:
	pass
```

Η αλλαγές που θα χρειαστεί το inventory είναι απλές, θα χρειαστούμε έναν τρόπο να βγάζουμε αντικείμενα από μέσα του. 
Στην αρχή ορίσαμε ένα function για να βάζουμε αντικείμενα τώρα ήρθε η ώρα για το αντίθετο.
Για να γίνει αυτό θα κάνουμε μερικές προετοιμασίες.

```gdscript
func OnItemAdded(It : Item) -> void:
	_CurrentWeight += It._ItemWeight
	OnWeightChanged.emit(_CurrentWeight)

func OnItemRemoved(It : Item) -> void:
	_CurrentWeight -= It._ItemWeight
	OnWeightChanged.emit(_CurrentWeight)
```

Πρώτον θα ετοιμάσουμε το OnItemRemoved μέσα στο Inventory, για να αλλάζουμε το βάρος ενημερώνουμε το UI οτι το βάρος άλλαξε.
Το επόμενο είναι οτι θα πρέπει να ετοιμαστεί η λογική του της διαγραφής ενός container. Θα πρέπει να ενημερώνουμε το UI οτι κάτι τέτοιο έγινε για να διαγράφει και αυτό τα δικά του container.


Ο τρόπος που θα το κάνουμε είναι οτι θα δημιουργήσουμε ένα signal μέσα στο container όπου το UIContainer θα εγγραφεί, και όταν το signal καλεστεί το UI θα διαγράψει τον εαυτό του.

```gdscript
extends Resource

class_name InventoryItemContainer

var _StoredItem : Item
var _Ammount : int = 0

signal OnAmmountUpdated(NewAmmount : int)
signal OnContainerEmptied

func RegisterItem(It : Item) -> void:
	_StoredItem = It

func UpdateAmm(Amm : int) -> void:
	_Ammount += Amm
	OnAmmountUpdated.emit(_Ammount)

func GetAmmount() -> int:
	return _Ammount
	
func GetContainedItem() -> Item:
	return _StoredItem

func ContainerRemoved() -> void:
	OnContainerEmptied.emit()
```

```gdscript
extends PanelContainer

class_name InventoryUIContainer

@export var Icon : TextureRect
@export var ItemAmmount : Label
@export var ItemName : Label

var _ItemCont : InventoryItemContainer

signal OnContainerSelected

func RegisterContainer(Cont : InventoryItemContainer) -> void:
	Cont.connect("OnAmmountUpdated", OnAmmountUpdated)
	Cont.connect("OnContainerEmptied", DeleteSelf)
	_ItemCont = Cont
	ItemAmmount.text = var_to_str(Cont.GetAmmount())
	ItemName.text = Cont.GetContainedItem().GetItemName()
	Icon.texture = Cont.GetContainedItem().GetItemIcon()

func OnAmmountUpdated(Amm : int) -> void:
	ItemAmmount.text = var_to_str(Amm)

func GetContainer() -> InventoryItemContainer:
	return _ItemCont

func _on_select_button_pressed() -> void:
	OnContainerSelected.emit()

func DeleteSelf() -> void:
	queue_free()
```

Μετά από αυτά τα βήματα θα στήσουμε το RemoveItem function χρησιμοποιώντας ότι ετοιμάσαμε μόλις.
Το function μας θα στηθεί κάπως έτσι.

```gdscript
func RemoveItemFromContainer(Cont : InventoryItemContainer) -> void:
	Cont.UpdateAmm(-1)
	OnItemDropped.emit(Cont.GetContainedItem())
	OnItemRemoved(Cont.GetContainedItem())
	if (Cont.GetAmmount() == 0):
		_InventoryContents.erase(Cont)
		Cont.ContainerRemoved()
```

Το container που θα είναι διαλεγμένο εκείνη την στιγμή που ο παίκτης πατήσει το Drop Item θα περαστεί σε αυτό το function, η ποσότητα του container θα μειωθεί, μετά θα καλέσουμε το OnItemRemoved για να ενημερωθεί το βάρος και τέλος κοιτάμε αν το container είναι άδειο, αν ναι τότε βάζουμε μπρός την διαδικασία διαγραφής του.
Σιγουρευόμαστε ότι το καινούριο function καλείτε όταν πατηθεί το κουμπί μέσα στο OnItemDroped μέσα στο InventoryScreen.

```gdscript
func OnItemDroped() -> void:
	Inv.RemoveItemFromContainer(SelectedContainer)
```

Όπως μπορούμε να δούμε καταφέρνουμε επιτυχώς να αφαιρέσουμε το αντικείμενο από το inventory αλλά παρατηρούμε ένα “bug” όταν η στοίβα αδειάσει τα option μένουν ακόμη ανοιχτά. Αυτό μπορεί να προκαλέσει περαιτέρω προβλήματα μιάς και τα κουμπιά θα προσπαθούν να εκτελέσουν επιλογές σε container που δεν υπάρχουν πια.

<img src="/assets/images/DropOption.gif" alt="Alt text" width="600" />

Για να λυθεί αυτό θα χρειαστεί το διαλεγμένο UIcontainer να ενημερώνει το InventoryScreen ότι επρόκειτο να διαγράψει τον εαυτό του και με την σειρά του το Inventory Screen να κλείνει το menu με τα option.

```gdscript
extends PanelContainer

class_name InventoryUIContainer

@export var Icon : TextureRect
@export var ItemAmmount : Label
@export var ItemName : Label

var _ItemCont : InventoryItemContainer

signal OnContainerSelected
signal OnContainerDeselected

func RegisterContainer(Cont : InventoryItemContainer) -> void:
	Cont.connect("OnAmmountUpdated", OnAmmountUpdated)
	Cont.connect("OnContainerEmptied", DeleteSelf)
	_ItemCont = Cont
	ItemAmmount.text = var_to_str(Cont.GetAmmount())
	ItemName.text = Cont.GetContainedItem().GetItemName()
	Icon.texture = Cont.GetContainedItem().GetItemIcon()

func OnAmmountUpdated(Amm : int) -> void:
	ItemAmmount.text = var_to_str(Amm)

func GetContainer() -> InventoryItemContainer:
	return _ItemCont

func _on_select_button_pressed() -> void:
	OnContainerSelected.emit()

func DeleteSelf() -> void:
	OnContainerDeselected.emit()
	queue_free()
```
Ορίζουμε λοιπόν το signal και το καλούμε στο DeleteSelf function.
Και τέλος ενημερώνουμε το OnItemContainerSelected έτσι ώστε να συνδέεται στο signal από το container που είναι διαλεγμένο, θα πρέπει να θυμόμαστε να ξε-συνδέουμε το signal όταν αλλάζουμε διαλεγμένο container και όταν ένα αντικείμενο ξε-διαλεχτεί.

```gdscript
func OnItemContainerSelected(ItemContainer : InventoryUIContainer) -> void:
	if (SelectedContainer != null):
		SelectedContainer.disconnect("OnContainerEmptied", OnItemContainerDeselected)
	SelectedContainer = ItemContainer.GetContainer()
	SelectedContainer.connect("OnContainerEmptied", OnItemContainerDeselected)
	ItemOptions.SetSelectedItem(SelectedContainer.GetContainedItem())

func OnItemContainerDeselected() -> void:
	SelectedContainer.disconnect("OnContainerEmptied", OnItemContainerDeselected)
	SelectedContainer = null
	ItemOptions.visible = false
```

Το επόμενο βήμα είναι το Inspect, θα ετοιμάσουμε για αυτό το UI για αρχή και μετά θα δούμε πως θα το αναλύσουμε. Θα γίνει κάτι παρόμοιο με το options menu οπότε η εμπειρία θα μας είναι πολύτιμη. Θα χρειαστούμε κάτι απλό, 2 label για το όνομα του αντικειμένου και την περιγραφή, Ενα TextureRect για το εικονίδιο του, και ένα κουμπί για να κλείνουμε το παράθυρο.  

<img src="/assets/images/InspectMenu.jpg" alt="Alt text" width="600" />
<img src="/assets/images/InspectionVisuals.jpg" alt="Alt text" width="600" />

Το script θα στυθεί κάπως έτσι, το μόνο που χρειαζόμαστε είναι ένα function για να περνάμε το αντικείμενο που θα κάνουμε inspect και μέσα του θα κάνουμε configure όλες τις πληροφορίες του. Και τέλος ένα function που θα καλεί το κουμπί για να κλείνει αυτό το παράθυρο.

```gdscript
extends Control

class_name InventoryScreen

@export var WeightText : Label
@export var ContainerPlacement : Control
@export var ItemOptions : UIItemOptions
@export var InventoryContainerScene : PackedScene
@export var Inv : Inventory
@export var InventoryDescriptorScene : PackedScene
```

Τέλος μέσα στο OnItemInspected που βρίσκεται μέσα στο Inventory Screen θα δημιουργούμε το scene αυτό και θα του περνάμε τα data.

```gdscript
func OnItemInspected() -> void:
	var Inspector = InventoryDescriptorScene.instantiate() as InspectMenu
	Inspector.SetItem(SelectedContainer.GetContainedItem())
	add_child(Inspector)
```

Τεστάρουμε και βλέπουμε οτι όλα δουλεύουν όπως πρέπει.

<img src="/assets/images/OptionMenuGif.gif" alt="Alt text" width="600" />