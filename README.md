# GenDC Tools

**GenDC (Generic Data Container)** is a flexible data format developed by the [GenICam ( Generic Interface for Cameras)](https://www.emva.org/standards-technology/genicam/) working group under the [European Machine Vision Association (EMVA)](https://www.emva.org/). 

It is designed to standardize the representation and transmission of various types of imaging data across different transport layers.​

The library provided in C++ and Python in the repository allows you to extract specific information and data from a GenDC container without needing an in-depth understanding of its internal structure or descriptor format.

* [Key Features of GenDC](#key-features-of-gendc)
* [Installation](#installation)
* [Quick start (Python)](#quick-start-python)

## Key Features of GenDC

### 1. Developed by EMVA's GenICam Working Group

GenDC is one of the GenICam modules developed by the EMVA, aiming to provide a generic programming interface for all cameras and imaging devices. 

### 2. ​Versatility in Data Format

GenDC supports a wide range of data formats commonly used in machine vision—such as 2D, 3D, multi-spectral, compressed formats (e.g., JPEG, H.264), and metadata like GenICam chunks, histograms and statistics. 

Beyond imaging, any type of data (including non-pixel based data) can be encapsulated in a GenDC container.

The structure of the container is fully defined in the Descriptor, allowing precise customization of how the data is organized and interpreted, regardless of its type.

### 3. Self-Describing File Structure

In GenDC, the structure of the container is explicitly defined using a Descriptor, which describes how the Data is organized, including the layout, types, and relationships of various components.

This self-describing approach allows any kind of data — images, metadata, sensor readings, etc. — to be included and interpreted correctly by receivers, even without prior knowledge of the specific data format.

### 4. Transport Layer Neutrality

GenDC is independent of the underlying transport protocol, meaning it can be used across various transport technologies such as GigE Vision, USB3 Vision, CoaXPress, and Camera Link HS.

This allows the same data format to be transmitted over different interfaces without modification, simplifying system integration and increasing flexibility.

### 5. Transport Layer Agnostic

The transport layer does not need to interpret the contents of the GenDC payload, as it is treated as an opaque data block.

## Installation

If you want to install a whole Sensing-Dev Software development kit (Sensing-Dev SDK) that has all components of camera aqcuisioon library and efficient image processing framework with API, follow the instruction on the [Sensing-Dec webpage](https://sensing-dev.github.io/doc/).

The simgle library of GenDC Tools for Python and C++ are availabe:

### Python
```bash
pip install gendc-python
```

### C++ Header Library

Download the latest release package in [Release](https://github.com/Sensing-Dev/GenDC/releases) for Linux and Windows.

## Quick start (Python)

Download sample GenDC binary file (`output.bin`) from [here](https://github.com/Sensing-Dev/GenDC/blob/main/test/generated_stub/output.bin) and load it as `binary_gendc_information`.

```python
from gendc_python.gendc_separator import descriptor

with open('output.bin', mode='rb') as ifs:
  binary_gendc_information = ifs.read()
  gendc_container = descriptor.Container(binary_gendc_information)

  if descriptor.is_gendc(gendc_container):
    print('This data is a valid GenDC data')
```

See more details in tutorials on the [Sensing-Dec webpage](https://sensing-dev.github.io/doc/).

## Quick start (C++ header library)

Adding an include directory to the compiler's search path using the `-I` flag.

```cpp
#include "gendc_common.h"

int main(int argc, char* argv[]){

}
```

##  GenDC Containers layout 
- **Container Header**
  - **Component 1 Header (RGB Planar Intensity)**
    - Part 1 Header
    - Part 2 Header
    - Part 3 Header
  - **Component 2 Header (Range)** 
    - Part 1 Header
  - **Component 3 Header (Metadata)**
    - Part 1 Header
      - Part 1 Data (Metadata Chunk)
- **Container Data**
  - Part 1 Data (Red plane)
  - Part 2 Data (Green plane)
  - Part 3 Data (Blue plane)
  - Part 1 Data (Range)
  - Part 1 Data (Metadata Chunk)

 **In this library, we reconstruct each element in following way:**

- **Container Header**
  - **Component 1 Header (RGB Planar Intensity)**
    - Part 1 Header
      - Part 1 Data (Red plane)
    - Part 2 Header
      - Part 2 Data (Green plane)
    - Part 3 Header
      - Part 3 Data (Blue plane)
  - **Component 2 Header (Range)**
    - Part 1 Header
      - Part 1 Data (Range)
  - **Component 3 Header (Metadata)**
    - Part 1 Header
      - Part 1 Data (Metadata Chunk)




## API

### Container
```
gendc_container = descriptor.Container("binary_gendc_information")
```

* `is_gendc_descriptor(binary_info)`: Checks if the descriptor in the binary data matches the specific signature for a GENDC descriptor.
  * Parameters:
    * binary_info (bytes): The binary data from which to load the signature.
  * Returns:
    * bool: True if the binary data is GenDC format

* `get_data_size()`: Retrieves the size in bytes of the data of the container.
  * Returns:
    * int: total size in bytes of the container data
* `get_descriptor_size()`: Retrieves the size in bytes of the descriptor part of the container.
  * Returns:
    * int: total size in bytes of the descriptor
* `get_container_size()`: Calculates the total size of the container by adding up data size and descriptor size.
  * Returns:
    * int: total size in bytes of the container 
* `get_component_count()`: Retrieves the count of components in the container.
  * Returns:
    * int: the count of components in the container.

*  `get_1st_component_idx_by_typeid(typeid)`: Finds the index of the first available component with the specified data type.
  * Parameters:
    * typeid (int): The target type ID to search for in components.
  * Returns:
    * int: The index of the first available component with the matching data type, or -1 if not found.

* `get_1st_component_idx_by_sourceid(sourceid)`: Finds the index of the first available component with the specified source ID.
  * Parameters:
    * sourceid (int): The target source ID to search for in components.
  * Returns:
    * int: The index of the first available component with the matching data type, or -1 if not found.

* `get_component_by_index(ith_component)`: Retrieves the component header at the specified index.
  * Parameters:
    * ith_component (int): The index of the component to retrieve.
  * Returns:
    * Component object: The component object at the specified index.

* `get_component_header_value(key, ith_component)`: Retrieves a specific value from a component's header based on a given key.
  * Parameters:
    * ith_component (int): The index of the component to retrieve.
    * key (str): The value from component header to retrieve.
  * Returns:
    * Any: The value associated with the key from the specified component's header..
* `get_part_header_value(key, ith_component, jth_part)`: Retrieves a specific value from a part's header within a component based on a given key.
  * Parameters:
    * ith_component (int): The index of the component to retrieve.
    * jth_part (int): The index of the part within the component.
    * key (str): The key of the value from part header to retrieve.
  * Returns:
    * Any: The value associated with the key from the specified part's header within the component.
* `get(key)`: General method to retrieve a specific value from a container's header based on a given key.
  * Parameters:
    * key (str): The key of the value from part header to retrieve.
  * Returns:
    * Any: The value associated with the key from container header.

### Component
* `get_part_count()`: Retrieves the count of parts in the component.
  * Parameters:
    * key (str): The key of the value from part header to retrieve.
  * Returns:
    * int: The number of parts.

* `get_part_by_index(jth_part)`: Retrieves a part based on its index.
  * Parameters:
    * jth_part (int): The index of the part to retrieve.
  * Returns:
    * Part Object: The part header at the specified index.

* `get(key)`: Retrieves a specific value from container header based on a given key.
  * Parameters:
    * key (str): The key of the value from part header to retrieve.
  * Returns:
    * Any: The value associated with the key from container header.

* `get_part_header_value(key, jth_part)`: Retrieves a specific value from a part's header based on a given key.
  * Parameters:
    * jth_part (int): The index of the part to retrieve.
  * Returns:
    * Any: The value associated with the key from part header.
* `get(key)`: General method to retrieve a specific value from current component header based on a given key.
  * Parameters:
    * key (str): The key of the value from current component header to retrieve.
  * Returns:
    * Any: The value associated with the key from current component header.

## Part

* `is_data_1D_image()`: Determines if the part data is a 1D image based on the header type.
  * Returns:
    * bool: True if the header type indicates a 1D image, otherwise False.
* `is_data_2D_image()`: Determines if the part data is a 2D image based on the header type.
  * Returns:
    * bool: True if the header type indicates a 2D image, otherwise False.
* `get_dimension()`: Retrieves the dimensions of the data.
  * Returns:
    * list: A list containing the dimensions of the part. Returns an empty list if neither.

* `get_data_size()`: Retrieves the size of the data.
  * Returns:
    * int: The size of the data part in bytes

* `get_data_offset()`: Retrieves the offset at which the data part starts within the binary file.
  * Returns:
    * int: The offset of the data part in bytes.

 * `get_data()`: Extracts and returns the data part based on its size and offset.
   * Returns:
     * bytes: The binary data of the part.

* `get(key)`: General method to retrieve a specific value from current part header based on a given key.
  * Parameters:
    * key (str): The key of the value from current part header to retrieve.
  * Returns:
    * Any: The value associated with the key from current part header.


## Reference

GenDC v1.1 by emva https://www.emva.org/wp-content/uploads/GenICam_GenDC_v1_1.pdf
