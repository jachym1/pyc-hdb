# pyc-hdb Python compiled HANA client

Procedure to compile pure SAP Python client for HANA Database to C binary executable to speed up program execution.

Install Python and Nuitka package from http://nuitka.net/pages/download.html
Edit fibers.h Nuitka header located in site-packages/nuitka/build/include/nuitka to avoid error during compilation (missing type)
Add line
#include <stdint.h>

Install latest py-HDB client version from git epository
git clone https://github.com/SAP/pyhdb.git

Edit pyhdb/pyhdb/protocol/types.py to avoid NoneType error during compilation
Comment lines:

class NoneType(Type):

    python_type = None.__class__

    @classmethod
    def to_sql(cls, _):

        return text_type("NULL")


    @classmethod

    def prepare(cls, type_code):

        """Prepare a binary NULL value for given type code"""
        
 This is achieved by setting the MSB of the type_code byte to 1

        return struct.pack('<B', type_code | 0x80)


file  __main__.py according your needs – HANA db connection parameters and sql statements

Compile the program

nuitka . --recurse-all
mv ..exe test
./test

Enjoy
