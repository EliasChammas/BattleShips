package 
{
	import flash.display.SpreadMethod;
	import flash.display.Sprite;
	import flash.events.Event;
	import flash.events.KeyboardEvent;
	import flash.events.MouseEvent;
	import flash.text.TextField;
	
	/**
	 * ...
	 * @author Elias
	 */
	public class Main extends Sprite 
	{
		public var map:Vector.<Vector.<Tile>> = new Vector.<Vector.<Tile>>();
		private var scoreText:TextField;
		private var score:Number;
		
		private var missText:TextField = new TextField();
		private var miss:Number;
		
		public function Main():void 
		{
			if (stage) init();
			else addEventListener(Event.ADDED_TO_STAGE, init);
		}
		
		private function init(e:Event = null):void 
		{
			removeEventListener(Event.ADDED_TO_STAGE, init);
			// entry point
			
			var pressSpace:TextField = new TextField();
			pressSpace.text = "Press Space";
			pressSpace.x = 350;
			pressSpace.y = 250;
			addChild(pressSpace);
			
			
			
			stage.addEventListener(KeyboardEvent.KEY_DOWN, keyDown);
		}
		private function newMap():void 
		{
			score = 0;
			scoreText = new TextField;
			scoreText.x = 50;
			addChild(scoreText);
			updateScore();
			
			miss = 0;
			missText = new TextField;
			missText.x = 150;
			addChild(missText);
			updateMiss();
			
			
			for (var i:int = 0; i < 10; i++) 
			{
				var enRad:Vector.<Tile> = new Vector.<Tile>();
				for (var j:int = 0; j < 10; j++) 
				{
					var t:Tile = new Tile();
					t.y = 100 + j * (Tile.TILE_SIDE + 1);	//We generate the 100 tiles
					t.x = 100 + i * (Tile.TILE_SIDE + 1);
					this.addChild(t);
					this.addEventListener(MouseEvent.CLICK, clickTheBox);
					enRad.push(t);
					
				}
				map.push(enRad);
				
 			}
		}
		
		private function updateMiss():void 
		{
			missText.text = "Misses: " + miss;
		}
		
		private function keyDown(e:KeyboardEvent):void 
		{
			if (e.keyCode == 32) //when we press the spacebar
			{	
				
				
				while (numChildren > 0) 
				{
					removeChildAt(0); //We delete everything on the stage
				}
				
				map.length = 0; //we empty our primary vector
				
				newMap(); //we generate a new game
				
				score = 0;
				scoreText.text = "Hit: 0";
											//Score points and miss marks gets reset
				miss = 0;
				missText.text = "Misses: 0"
				
				putShipDown(3);
				putShipDown(3);	// We give the amount of tiles we want our ship to cover 
				putShipDown(3);
				
				var Elias:TextField = new TextField();
				Elias.text = "Made By Elias";
				Elias.x = 600;
				Elias.y = 550;
				addChild(Elias);
			}
		}
		private function clickTheBox(e:MouseEvent):void 
		{
			Tile(e.target).clicked(); //We are acsessing our class to get our tiles that is our targets
			
			if (Tile(e.target).scoreOrNot) 
			{
				score++;
				updateScore();//If we hit, we get a point
			}else 
			{
				miss++;
				updateMiss(); //if we miss, we will get a mark for missing
			}
			
			
			
		}
		private function updateScore():void 
		{
			scoreText.text = "Hit: " + score;
		}
		private function putShipDown(shipLength:int = 3):void
		{
			var horizantalOrNot:Boolean = false;
			var shipXpos:int;
			var shipYpos:int;
			var currentShip:Vector.<Tile> = new Vector.<Tile>();
			if (Math.random() > 0.5)
			{
				horizantalOrNot = true;
			}
			if (horizantalOrNot)
			{
				shipXpos = Math.random() * (map.length-shipLength);
				shipYpos = Math.random() * (map.length - 1);	//This will tell us if our ship will be placed horizontal or vertical
				for (var i:int = 0; i < shipLength; i++) 
				{
					currentShip.push(map[shipXpos+i][shipYpos]);
				}
			}
			else
			{
				shipXpos = Math.random() * (map.length - 1);
				shipYpos = Math.random() * (map.length-shipLength);
				for (var i:int = 0; i < shipLength; i++) 
				{
 					currentShip.push(map[shipXpos][shipYpos+i]);
				}
			}
			for (var j:int = 0; j < currentShip.length; j++) 
			{
				if (currentShip[j].isShip)
				{
					putShipDown(shipLength);
					return;
				}
			}
			for (var k:int = 0; k < currentShip.length; k++) 
			{
				currentShip[k].isShip = true;
			}
		}
		
		//This is a code for the game Battleships. It runs without any sort of problems is well written and in the right formats.
		//About 99 % of the code is written in camelcase with a few exceptions, for reading purposes.
		//I think that the code is easy to understand, again for exception for the Vectors used in the code.
		//The code is commented to tells us what the code actually does
		
	}
	
}


__________________________
min tile class kommer här under

____________________________

	package  
	{
	import flash.display.Bitmap;
	import flash.display.Sprite;
	/**
	 * ...
	 * @author Elias
	 */
	




	public class Tile extends Sprite
	{
		[Embed(source = "Vatten1.png")]
		private var WaterImage:Class;
		
		[Embed(source = "VattenHit.png")]
		private var WaterHit:Class;
		
		[Embed(source = "VattenMiss.png")]
		private var WaterMiss:Class;
		
		
		public static const WATER:int = 1;
		public static const WATERHIT:int = 2;
		public static const WATERMISS:int = 3;
		public var isShip:Boolean = false;
		public var BattleShip:int = 3;
		
		public static const TILE_SIDE:int = 48;
		private var typeOfTerrain:int = 0;
		
		public var scoreOrNot:Boolean;
		
		
		
		public function Tile() 
		{
			
			this.graphics.beginBitmapFill(Bitmap(new WaterImage()).bitmapData);
			this.graphics.drawRect(0, 0, TILE_SIDE, TILE_SIDE);
			this.graphics.endFill();
		}
		
		public function clicked():void 
		{
		
			if (this.isShip == true) 
			{
				this.isShip = BattleShip;
				this.graphics.clear();
				this.graphics.beginBitmapFill(Bitmap(new WaterHit()).bitmapData);
				this.graphics.drawRect(0, 0, TILE_SIDE, TILE_SIDE);
				this.graphics.endFill();
				scoreOrNot = true;
				
				if (scoreOrNot = true) 
				{
					getScore();
				}
				
				
			}
			else 
			{
				this.graphics.clear();
				this.graphics.beginBitmapFill(Bitmap(new WaterMiss()).bitmapData);
				this.graphics.drawRect(0, 0, TILE_SIDE, TILE_SIDE);
				this.graphics.endFill();
				
				scoreOrNot = false;
				
				if (scoreOrNot = false) 
				{
					markMiss();
				}
			}
			
			
		}
		
		public function getScore():int 
		{
			return + 1;
		}
		
		public function markMiss():int 
		{
			return + 1;
		}
		
		
		
	}

}


