
<p style="font-size:14px" align="center">
<a href="https://m.do.co/c/2cea00d4f9bble" target="_blank">Deploy your VPS using our referral link to get 100$ free bonus for 60 days <img src="https://user-images.githubusercontent.com/50621007/183284313-adf81164-6db4-4284-9ea0-bcb841936350.png" width="30"/></a>
</p>

<p align="center">
  <img height="50" height="auto" src="https://user-images.githubusercontent.com/38981255/184088981-3f7376ae-7039-4915-98f5-16c3637ccea3.PNG">
</p>

<p style="font-size:30px" align="center"> Tutorial Become a Master Node Inery Blockchain Task 3

Dokumen Innery Official :
> [Node Lite & Master](https://docs.inery.io/docs/category/lite--master-nodes)

Explorer Inery BlockChain :

> [Explorer Inery BlockChain](https://explorer.inery.io/ "Explorer Inary")

# Inery Task 3 Update
Because Task 2 has been approved, let's move on to the next step, task 3

**First of all, check if your account has been approved?**

> NOTE: If you have problems logging in, use the incognito tab in your browser

<p align="center">
  <img height="auto" height="auto" src="https://user-images.githubusercontent.com/112532410/200989702-859210c3-a449-47bd-a207-3a31dc1f9852.jpg">
</p>

- OKAY, Time to go to task 3
## Create Your Value Contract
- Install Package
```
wget -qO crud.sh https://raw.githubusercontent.com/0xevoid/inery-3/main/crud.sh && chmod +x crud.sh && ./crud.sh
```
- Set Path
```
export PATH="$PATH:$HOME/inery.cdt/bin:$HOME/inery-node/inery/bin"
```
- Create Folder
```
mkdir -p $HOME/inrcrud
```
- Write Code
```
sudo tee $HOME/inrcrud/inrcrud.cpp >/dev/null <<EOF
#include <inery/inery.hpp>
#include <inery/print.hpp>
#include <string>

using namespace inery;

using std::string;

class [[inery::contract]] inrcrud : public inery::contract {
  public:
    using inery::contract::contract;


        [[inery::action]] void create( uint64_t id, name user, string data ) {
            records recordstable( _self, id );
            auto existing = recordstable.find( id );
            check( existing == recordstable.end(), "record with that ID already exists" );
            check( data.size() <= 256, "data has more than 256 bytes" );

            recordstable.emplace( _self, [&]( auto& s ) {
               s.id         = id;
               s.owner      = user;
               s.data       = data;
            });

            print( "Hello, ", name{user} );
            print( "Created with data: ", data );
        }

         [[inery::action]] void read( uint64_t id ) {
            records recordstable( _self, id );
            auto existing = recordstable.find( id );
            check( existing != recordstable.end(), "record with that ID does not exist" );
            const auto& st = *existing;
            print("Data: ", st.data);
        }

        [[inery::action]] void update( uint64_t id, string data ) {
            records recordstable( _self, id );
            auto st = recordstable.find( id );
            check( st != recordstable.end(), "record with that ID does not exist" );


            recordstable.modify( st, get_self(), [&]( auto& s ) {
               s.data = data;
            });

            print("Data: ", data);
        }

            [[inery::action]] void destroy( uint64_t id ) {
            records recordstable( _self, id );
            auto existing = recordstable.find( id );
            check( existing != recordstable.end(), "record with that ID does not exist" );
            const auto& st = *existing;

            recordstable.erase( st );

            print("Record Destroyed: ", id);

        }

  private:
    struct [[inery::table]] record {
       uint64_t        id;
       name     owner;
       string          data;
       uint64_t primary_key()const { return id; }
    };

    typedef inery::multi_index<"records"_n, record> records;
 };
EOF
```
- Compile Code
```
inery-cpp $HOME/inrcrud/inrcrud.cpp -o $HOME/inrcrud/inrcrud.wasm
```
- Open Wallet
```
cline wallet unlock --name Your_Wallet_Name --password Your_Wallet_Password
```
- Set Contract
```
cline push action Your_account_name create '[1, "Your_account_name", "My Record"]' -p Your_account_name --json
```
- Read Contract
```
cline push action Your_account_name read [1] -p Your_account_name --json
```
- Update Contract
```
cline push action Your_account_name update '[ 1,  "My Record Modified"]' -p Your_account_name --json
```
- Destroy Contract
```
cline push action Your_account_name destroy [1] -p Your_account_name --json
```
✅ Done
> Go to Your Inery Account Click Task 3 Click Finish and wait for it to be APPROVED
