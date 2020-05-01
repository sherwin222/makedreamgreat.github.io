

## About LastChance

LastChance is just a game for blockchain,please participate in moderation.

## Rule analysis
### General distribution rules
* Direct push 1 person gets 3 generations, 5 people get 10 generations, 10 people get 15 generations
* Generation 1 is 20%, Generation 2 is 6%, Generation 3-10% is 3%, Generation 11 is 5%, Generation 12 is 8%, Generation 13 is 10%, Generation 14 is 12%, Generation 15  is 15%

* 
### How to start?
* Before participating in the game, you must register 0.1 ETH, only need to register once, the recommender must be determined at the time of registration, and cannot be changed later
* Registration fee 0.1 ETH will be 100% allocated to the recommender according to the general allocation rules

### Block one

* 30% is allocated to the 1-15th generation of recommenders according to the general allocation rules
* Statically allocated according to the number of investments, 1ETH = 1.5% / day, 2ETH = 1.7% / day, 3ETH = 2% / day, 5ETH = 1.4% / day, 10ETH = 1.2% / day
* Dynamic plus static 2 times out, can be reinvested after out
* Platform service fee 7%

### Block two
* The ether participating in the 2 plates must be extracted from the 1 plate, and the total cannot exceed the total withdrawal amount, and the 2 plates will no longer be counted
* Investment range is 1-10ETH
* The principal can be withdrawn at any time, and no income will be generated after withdrawal
* Dynamic plus static double out, without principal
* The amount of 100% of the daily dividend income is distributed to the 1-15th generation of recommenders according to the general distribution rules
* Platform service fee 7%

### Source code analysis
```javascript
library SafeMath {
    
    function mul(uint256 a, uint256 b) 
        internal 
        pure 
        returns (uint256 c) 
    {
        if (a == 0) {
            return 0;
        }
        c = a * b;
        require(c / a == b, "SafeMath mul failed");
        return c;
    }

    function div(uint256 a, uint256 b) 
        internal 
        pure 
        returns (uint256) 
    {
        // assert(b > 0); // Solidity automatically throws when dividing by 0
        uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold
        return c;
    }

    /**
    * @dev Subtracts two numbers, throws on overflow (i.e. if subtrahend is greater than minuend).
    */
    function sub(uint256 a, uint256 b)
        internal
        pure
        returns (uint256) 
    {
        require(b <= a, "SafeMath sub failed");
        return a - b;
    }

    /**
    * @dev Adds two numbers, throws on overflow.
    */
    function add(uint256 a, uint256 b)
        internal
        pure
        returns (uint256 c) 
    {
        c = a + b;
        require(c >= a, "SafeMath add failed");
        return c;
    }
    
    /**
     * @dev gives square root of given x.
     */
    function sqrt(uint256 x)
        internal
        pure
        returns (uint256 y) 
    {
        uint256 z = ((add(x,1)) / 2);
        y = x;
        while (z < y) 
        {
            y = z;
            z = ((add((x / z),z)) / 2);
        }
    }
    //10000×(1-5%)^180≈0.98
    /**
     * @dev gives square. multiplies x by x
     */
    function sq(uint256 x)
        internal
        pure
        returns (uint256)
    {
        return (mul(x,x));
    }
    
    /**
     * @dev x to the power of y 
     */
    function pwr(uint256 x, uint256 y)
        internal 
        pure 
        returns (uint256)
    {
        if (x==0)
            return (0);
        else if (y==0)
            return (1);
        else 
        {
            uint256 z = x;
            for (uint256 i=1; i < y; i++)
                z = mul(z,x);
            return (z);
        }
    }
}

contract LastChanceGame {

	function getCompressData(uint256 _pID)
		private
		view
		returns(uint256,uint256)
	{
		uint256 _tmpCompressData = players[_pID].compressData;
		return (
	  		_tmpCompressData/10%10,
	  		_tmpCompressData/100%10000000000000000000000
	  	);
	}
		function isHavePlayer()
		view
		public
		returns(bool)
	{
		return xsIndex < players.length;
	}

	function genPlayerRemain()
		view
		public
		returns(uint256)
	{
		return (players.length - xsIndex);
	}
	function calc(uint256 _day, uint256 _fixResRate0000, uint256 _validJoin)
		private
		view
		returns(uint256)
	{
		uint256 _teth = _validJoin;
		if(_teth <= 1)
		{
			return (0);
		}
		uint256 _rndrate;
		if(_fixResRate0000 != 0)
			_rndrate = _fixResRate0000;
		else 
			_rndrate = genPercent(_teth);

		uint256 _income = _teth.mul(_rndrate).mul(_day).div(10000);
		/*uint256 _ret;
		if(_canIncome < _income){
		_ret = _canIncome;
		}else{
		_ret = _income;
		}*/
		return (_income);
	}
    function genPercent(uint256 _validJoin)
  		private
  		view
  		returns(uint256)
  	{
  		//
  		uint256 _index = _validJoin <= 1 ether ? 0 
  						: _validJoin <= 2 ether ? 1 
  						: _validJoin <= 4 ether ? 2
  					    : _validJoin <= 9 ether ? 3
  					    : 4;
        return (genPercents[_index]);
  	}
}


```

## open source license


