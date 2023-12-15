# 한글 3byte
```js
const getByteLengthOfString = function(s,b,i,c) {
	for(b=i=0; c=s.charCodeAt(i++); b+=c>>11?3:>>7?2:1);
	return b;
}

//TEST
const test = '테스트';
console.log('length : ', getByteLengthOfString(test), Bytes)// length : 9 Bytes
```

# 한글 2byte
```js
String.prototype.getBytes = function() {
	const contents = this;
	let str_character;
	let int_char_count = 0;
	let int_contents_length = contents.length;

	for(k=0; k<int_contents_length; k++){
		str_character = contents.charAt(k);
		if(escape(str_character).length > 4){
			int_char_count += 2;
		} else {
			int_char_count++;
		}
	}
	return int_char_count;
}

//TEST
const test = '테스트';
console.log('length : ', test.getBytes(), 'Bytes'); // length : 6 Bytes
```

# 입력된 길이만큼 문자열 반환
```js
String.prototype.getStringFromByteLength = function() {
	const contents = this;
	let str_character;
	let int_char_count = 0;
	let int_contents_length = contents.length;

	for(k=0; k<int_contents_length; k++){
		str_character = contents.charAt(k);
		if(escape(str_character).length > 4){
			int_char_count += 2;
		} else {
			int_char_count++;
		}

		if(int_char_count > length) {
			break;
		}

		returnValue += str_character;
	}
	return returnValue;
}
```
