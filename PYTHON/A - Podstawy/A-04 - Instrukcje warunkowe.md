**IF ELSE**
`if a>b:`
	instrukcja A
`elif a<b:`
	instrukcja B
`else:`
	pozostałe instrukcje

W momencie spełnienia warunku, kolejne instrukcje nie są już sprawdzane (program oszczędza pamięć) i wykonuje tylko tą jedną. prawidłową instrukcję.

**MATCH CASE**
`light = input("What's the light?")
`match light:
	`case 'green':
		`print("You can Go!)
	`case 'yellow':
		`print("warning!")
	`case 'red':
		`print ("STOP!")
	`case _:
		`print("ERROR!")

