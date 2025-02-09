---
title: "Inventory Tutorial"
excerpt_separator: "<!--more-->"
categories:
  - Blog
tags:
  - Post Formats
  - readability
  - standard
link: https://github.com/DonKoutsou/Inventory_Tutorial
Gameic : https://game-icons.net/
---

GIT REPO : [link](#)

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
-Επιθεώρηση ή αλλιώς inspect. Άνοιγμα παραθύρου με περιγραφή αντικειμένου που προβάλει visual 2D ή 3D. 
-Κατανάλωση ή consume 
-άφησε ή drop
-άκυρο cancel

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
[Gameic](#)

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

Η λίστα άδεια και με μερικά element
<div style="display: flex; justify-content: space-between;">
  <div style="flex: 1; text-align: center;">
    <img src="/assets/images/InventoryScreenVisualEmpty.jpg" alt="Image 1" style="max-width: 100%; height: auto;">
  </div>
  <div style="flex: 1; text-align: center;">
    <img src="/assets/images/InventoryScreenVisualFilled.jpg" alt="Image 2" style="max-width: 100%; height: auto;">
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

<img src="/assets/images/OptionMenuGif.gif" alt="Alt text" width="600" />
