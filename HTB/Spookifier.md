# Spookifier — HTB Very Easy

Date: 2026-04-23
Time spent: 3 個小時
Category (actual): Web (SSTI — Server-Side Template Injection)
Outcome: Solved with coaching

## 我試過什麼 & 結果

- CMDi → 純字串輸出
- URL目錄遍歷 → 跳回 http://URL/flag.txt
- 在輸入框輸入<%print("hello")%> → Internal Error
- 在輸入框輸入${open("../../../flag.txt").read()} → Win!

## 漏洞機制

這個漏洞是來自於他的原始碼調用的 mako.template 套件，漏洞發生在這裡
def generate_render(converted_fonts):
	result = '''
		<tr>
			<td>{0}</td>
        </tr>
        
		<tr>
        	<td>{1}</td>
        </tr>
        
		<tr>
        	<td>{2}</td>
        </tr>
        
		<tr>
        	<td>{3}</td>
        </tr>

	'''.format(*converted_fonts)
	
	return Template(result).render()
正常來講我的輸入會被直接轉換成他指定的格式，但是他有一個Template的動作，我可以利用mako的特性使用${open("../../../flag").read()}的方式讓其return出我想讓他執行的指令，至於為什麼，是因為他的font4(這裡的{3})就是包含所有的ASCII字元，他轉換完之後還是原格式，所以可以攻擊成功

## 下次看到類似模式，我會先做什麼

我會先檢查我看不懂的代碼，以及沒見過的套件的用途

## Tags
#ssti #mako #server-side-template-injection #flask 
#template-engine #python-web