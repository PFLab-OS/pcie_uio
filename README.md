# pcie_uio
Support library for uio_pci_generic device drivers.

## Usage
### Setup for new repository.
- Create your own repository.
- Add Makefile with following recipe: (example from [liva/xhci_uio](https://github.com/liva/xhci_uio))
  - `TARGET_KEYWORD` is used to look up a target device from `lspci -v` outputs.
  - `TARGET_DEFAULT_DRIVER` is used by `make restore` command to give control of a device to default kernel driver.
  - `ARGS` is passed to a.out when `make run`.
```
OBJS= main.o keyboard.o xhci.o usb.o hub.o
TARGET_PCI_BUS_ID=              # Leave blank unless you want to specify device manually.
TARGET_KEYWORD=XHCI             # This will be overrided by TARGET_PCI_BUS_ID if specified.
TARGET_DEFAULT_DRIVER=xhci_hcd
ARGS=

-include pcie_uio/common.mk
```
- Run following command to add this repo as a submodule.
```
git submodule add https://github.com/PFLab-OS/pcie_uio.git
```

### Run
- `make check` to ensure the device is there.
- `make load` to give a device control to uio_pci_generic.
- After that, `make run` to start and test your driver.
- `make restore` may be able to restore the control to `TARGET_DEFAULT_DRIVER`, or it may cause a kernel panic.

### common.mk parameters
- Specify `TARGET_PCI_BUS_ID` when `TARGET_KEYWORD` is not worked or ambiguous (e.g. multiple devices). example: `02:00.0`

### Examples
There are some examples using this library:
- [liva/xhci_uio](https://github.com/liva/xhci_uio)
- [hikalium/nvme_uio](https://github.com/hikalium/nvme_uio)
