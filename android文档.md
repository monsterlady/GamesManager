* ListActivity������һ�����õ�Activity��������ʾ��Ϸ�б����DLC�б�app����ʱĬ�Ͻ���ListActivity��������ʾ��Ϸ�б�

# ��Activity֮�䴫�����ݣ�
## ��activity
1. ����intent����2��������Ҫ���õ���activity����

``` java
Intent intent = new Intent(this, GameDetailActivity.class);
```

2. ��intent�з����ݣ������ڼ�ֵ�ԣ���1���������ַ������͵�Key����2�����������ǻ������͡�String����������ʵ��Serializable�ӿڵ��ࡣ��ΪҪ����Game��DLCInfo�������������඼ʵ����Serializable

``` java
intent.putExtra(MSG_ITEM, new Game());
```

3. ������activity

``` java
startActivity(intent);
```

### ���Ҫ��ȡ��activity�����������ݣ�

������������activity�Ĵ���ĳ���������2������CODE_ADD_GAME��һ��int����ΪrequestCode����ʶ��������activity��Ŀ��

``` java
startActivityForResult(intent, CODE_ADD_GAME);
```

#### ��ȡ���ݣ�
requestCode����֮ǰ��activity�����ģ������������ĸ���activity����
resultCode����activity�ķ��ش���
data����activity���ص�����

```java
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        switch (requestCode) {
            case CODE_ADD_GAME:
                if (resultCode == RESULT_OK) {
                    Game returnedGame = (Game) data.getSerializableExtra(MSG_RETURN_DATA);
                    this.games.add(returnedGame);
                    this.updateAndSave();
                }
                break;
            }
        }
    }
```

## ��activity
### ��ȡ���ݣ�
1. ��ȡ�����intent

```java
Intent intent = getIntent();
```

2. ��intent�л�ȡ����
```java
String type = intent.getStringExtra(REQUEST_TYPE);
currentGame = (Game) intent.getSerializableExtra(MSG_ITEM); //���ƵĻ���getIntExtra�ȵ�
```

### ���ص���activity��
1. ����intent
```java
Intent intent = new Intent();
```

2. ����resultCode

```java
setResult(RESULT_OK, intent);
```

3. ���б�ҪҲ���Է�����
```java
intent.putExtra(MSG_RETURN_DATA, currentGame);
```

4. ���ٵ�ǰ��activity

```java
finish();
```


# lambda���ʽ

�ο���http://www.runoob.com/java/java8-lambda-expressions.html

Java���о�������ôд��
``` java
button.setOnClickListener(
    new View.OnClickListener() {
        @Override
        public void onClick(View view) {
            Intent intent = new Intent();
            setResult(RESULT_CANCELED, intent);
            finish();
        }
    }
);
```

��java8�п�����ôд��
``` java
button.setOnClickListener(
    (View view) -> {
        Intent intent = new Intent();
        setResult(RESULT_CANCELED, intent);
        finish();
    }
);
```

������д����������һ���ģ�ֻ���ܹ����õ���롣

lambda���ʽ���Խ�һ���򻯣�
``` java
button.setOnClickListener(
    view -> {
        Intent intent = new Intent();
        setResult(RESULT_CANCELED, intent);
        finish();
    }
);

// �Ѻ������װ��һ������
button.setOnClickListener(
    view -> {
        DlcEditActivity.this.clickCancel(view);
    }
);

// �ռ���
button.setOnClickListener(this::clickCancel);
```

# RecyclerView

## Adapter (MyAdapter.java)

### ItemView
adapter������Ҫ��һ��ViewHolder�࣬����MyAdapter.java���

``` java
public class ItemView extends RecyclerView.ViewHolder
```

recycler view��һ��**item**��Ӧһ��**ItemView**��Ӧһ��**item.xml��layout**

��Adapter���н�layout��ItemView�󶨣�
``` java
    @Override
    public ItemView onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        View v = LayoutInflater.from(parent.getContext()).inflate(R.layout.item, parent, false);
        return new ItemView(v);
    }
```
�����layout֮��ItemView�Ϳ���ͨ��findViewById���layout�е�Ԫ�أ���onBindViewHolder���������ͨ��ItemView���޸���ЩԪ�ء�

### factory method

��ΪDLC�б����Ϸ�б��������в�ͬ��������������factory method������adapter

``` java
public static MyAdapter ForGames(List<Game> games, ListActivity.ItemClickListener callback)
public static MyAdapter ForDlcs(List<DlcInfo> dlcs, ListActivity.ItemClickListener callback)
```

### ItemClickListener

����б��е�ĳ����Ϸʱ������Ҫ��GameDetailActivity������Ϸ��gameList�е�index������Ĭ�ϵ�OnClickListenerֻ��һ��View�Ĳ��������������Զ�����һ���ӿ�����װһ�¡�

ListActivity�����ｫ�����崫��
``` java
adapter = MyAdapter.ForGames(this.games, this::clickViewGame);
```

MyAdapter�յ�֮�󣬴�ŵ�this.callback��
``` java
// 
public static MyAdapter ForGames(List<Game> games, ItemClickListener callback) {
    MyAdapter a = new MyAdapter();
    a.games = games;
    a.dlcs = null;
    a.callback = callback;
    a.dlcMode = false;
    return a;
}
```

����װһ�㴫��OnClickListener��ʵ�ʵ��õ���ListActivity���clickViewGame����


``` java            
holder.itemView.setOnClickListener(v -> this.callback.Invoke(position));
```