- 👋 Hi, I’m @codfinder
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
codfinder/codfinder is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
with pd.ExcelWriter('D:\data\class.xlsx') as wt:
    for key, value in df.items():
        cols = [x for x in value.columns if x.find('姓名') >= 0]
        tmp = (value.assign(new_name=value[cols].apply(lambda x: x.to_list(), axis=1))
            .explode('new_name')
            .dropna(subset='new_name')
            .assign(number=value['reguer'].apply(lambda x: int(re.findall(r'\d', x)[0])))
        )[['product', 'dept', 'classes', 'new_name', 'reguer', 'number']]

        tmp.loc[len(value.index),'number'] = tmp['number'].sum()
        tmp.loc[len(value.index),'product'] = 'total'
        tmp.to_excel(wt,sheet_name=f'{key}resizer',index=False)
