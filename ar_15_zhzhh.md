###|&与或的中缀表达式转后缀表达式
& 的优先级高于 |

``` php
<?php 
# new php file
# please using & |
function trans($exp){
	$exp = trim($exp,'|& ');
	$exp = str_replace(' ','',$exp);
	$retVal = array();
	$exp_array = str_split($exp);
	$transExp = array();
	$tmp = '';
	foreach($exp_array as $char){
		$char = strval($char);
		if($char == '('){
			array_push($retVal,$char);
		}else if($char == ')'){
			if($tmp != ''){
				array_push($transExp,$tmp);
				$tmp = '';
			}
			while($retVal && (end($retVal)) != '('){
				array_push($transExp,array_pop($retVal));
			}
			if(end($retVal) == '('){
				array_pop($retVal);
			}
		}else if($char == '|' || $char == '&'){
			if($tmp != '') {
				array_push($transExp,$tmp);
				$tmp = '';
			}
			if(end($retVal) && end($retVal) != '(' && compare($char,end($retVal))){
				while($retVal && compare($char,end($retVal))){
					array_push($transExp,array_pop($retVal));
				}
			}
			array_push($retVal,$char);
		}else if(($char>='0' && $char<='9' ) || $char == '.'){
			$tmp .=$char;
		}
	}
	if(!empty($tmp)){
		array_push($transExp,$tmp);
	}
	while ($row = array_pop($retVal)){
		array_push($transExp,$row);
	}
	return $transExp;
}

function compare($current,$a){
	if($current == '|'){
		return 1;
	}else{
		if($a=='&'){
			return 1;
		}else{
			return 0;
		}
	}
}

echo implode(' ', trans("2.43"));
//1.2 & 2.1 & ( 2.3 | 2.4) | (1.3&2.2) 输出 1.2 2.1 & 2.3 2.4 |  & 1.3 2.2 & |
//1.2 &  ( 2.3 | 2.4) | (1.3&2.2)   输出 1.2 2.3 2.4 |  & 1.3 2.2 & |

?>

```
